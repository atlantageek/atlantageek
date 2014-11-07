---
layout: post
title: "Detecting Seasonality"
categories:
- programming
- data
- R
tags:
- R
comments: true
---
Automated way to detect Seasonality
=========

When working with real-world data its obvious that data does not arrive in the smooth consistent nature that your linear regression textbook suggested. Data comes in fits and starts. It comes only once a week or 3 days a week. Its sent at 11:55 PM and in the process crosses the day boundary. It reacts to holidays either taking them off or doubling in quantity. This makes forecasting and anomoly detection at the large scale difficult but not impossible.

In the previous [article](http://atlantageek.com/2014/10/09/detecting-cadence/) we discussed how to identify when data is not sent by detecting its cadence. My current project has 1000s of data streams delivered every month and we needed to identify when the stream does is not sent. However data being sent is not the only problem. Another issue is that not all the data has been sent. To identify anomolies in the data streams we need to know how much data is sent how often.  We need to be able to say that 'data stream 1 sends 7,000 records every week' or 'data stream 2 sends 1000 records every 2 weeks' even if the majority of those records are sent on the last friday.

To make these type of statements we need to:
1. Identify the data cycle. (week, month, bi-weekly)
2. aggregate the data up to the cycle and base the forecast on the cycle type period.

The rest of this post will focus on item 1. Below is 200 days of data for one of the streams.

![sales data](/img/salesdata.png)

If you look closely you would identify that this data comes in weekly cycles but almost every day some data comes in. How can we identify programatically the cycle of the data. The approach taken here is to use correlate between the current data and lagged data. Lagged data being data shifted by days. To calculate this we use the following R script.

        data <- read.csv("a.csv")

        lagpad <- function(x, k=1) {
                i<-is.vector(x)
                if(is.vector(x)) x<-matrix(x) else x<-matrix(x,nrow(x))
                if(k>0) {
                        x <- rbind(matrix(rep(NA, k*ncol(x)),ncol=ncol(x)),
                         matrix(x[1:(nrow(x)-k),], ncol=ncol(x)))
                }
                else {
                        x <- rbind(matrix(x[(-k+1):(nrow(x)),],
                         ncol=ncol(x)),matrix(rep(NA, -k*ncol(x)),ncol=ncol(x)))
                }
                if(i) x[1:length(x)] else x
        }

        c <- c() #Initialize Correlation vector.
        
        #Try lagging from 1 to 32 days and see which has the strongest correlation.
        for (i in 1:32) 
        {
                c[i] <- cor(data$val, lagpad(data$val,i), use="complete.obs")
        }
        barplot(c)


Here we are looping thrugh lags 1-32 and correlating the data with each lag.  A single correlation value is generated for each lag value. We can plot those below

![correlation](/img/correlation.png)

The image shows that at 7,14,21,28 day lags are the strongest correlation.  If data is strongly correlated every 7 days then multiples of 7  would be strongly correlated too. This presents another problem.  Looking at this graph you would see the 7 day cycle and the slightly stronger correlation at the 14 day mark could be considered an accident. However because of this we cant just take the largest value for the cycle. The lower factors should get precedence over the larger multiples even if the multiples have higher correlation.

The correlation can be adjusted by subtracting out the factors correlation. For instance how much would be left if the 7 day correlation was subtracted out of the 14 day correlation. We will clean up the graph by zeroing out the negative correlations also. We append the following to the previous script to get an adjusted dataset.

     adj_corr <- c
     for (i in 1:length(c))
     {
       if (c[i] > 0) {
         factors <- unique(factorize(i))
          print(factors)
         for (j in 1:length(factors))
         {
           k = as.numeric(factors[j])
           if (i != k)
           {
             if ((adj_corr[i] < adj_corr[k]) && (adj_corr[k] > 0 ) )
             {
               adj_corr[i] <- 0
             }
             else
             {
               adj_corr[i] <- adj_corr[i] - adj_corr[k]
             }
           }
         }
       }
       else
       {
         adj_corr[i] = 0
       }
     }
     

This results in the following adjusted correlation

![correlation](/img/adj_correlation.png)

The correlation for 14,21 and 28 days is much weaker when we take the 7 day correlation out.

Based on this graph we realized that we need to aggregate up 7 days of data to generate forecasts. This can be used to identify bi-weekly cycles or twice a week cycles or once a quarter data if you have that much data.

This was a simple case but what does a monthly cycle look like.

![correlation](/img/monthly_correlation.png)

In this case the data is send on the first of every month.

Here we can see that the uneven number of days in a month are causing issues. And without a single strong correlation like the data will vary wildly. For example if we aggregate up at the 30 day time period. There will be some periods where we will go 30 days without any data.  And at least 1 30 day period (the one that contains feburary) a year will have twice as much data correlated to it.If we increas the period by 1 day then 6 periods a year will have twice as much data.

If your decisions are on a daily basis and you can handle this sort of jumps in your forecast then your algorithm needs to be calendar aware.

Other than monthly data this is a good technique to identify aggregation periods automatically. 


