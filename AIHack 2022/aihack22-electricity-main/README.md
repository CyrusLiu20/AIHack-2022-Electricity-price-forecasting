# AI-hack 2022, Electricity

![alt](limejump-logo.png)


# Problem description


In the UK, the price of electricity changes every 30 minutes.

We provide some time series data sets which describe this power price over the last few years. 

There is the EPEX price, the intraday price, and the system price; which correspond to 3 slightly different markets on which one can ‘buy’ and ‘sell’ power. These are explained in more detail below.

We suggest taking two approaches with this dataset:
1)	Forecasting prices using some time series methods and possible some machine learning models

2)	Creating an optimisation for a flexible asset, such as a battery, to take advantage of price differentials at different times of day.

It’s up to you which route you take! In reality, you’d need to produce a price forecast against which you would optimise an asset, so depending on how much time you have, the both can be rolled in the same challenge. Reinforcement learning could be an option here.  

If you decide to forecast prices, you may want to supplement the dataset with additional public, open sources of data; for example system demand, and temperature or weather data; a good place to find this data is bmreports.com, eg 
https://www.bmreports.com/bmrs/?q=demand/rollingsystemdemand/historic
Shows the 5 minute demand in real time as measured in the National Grid control room. 
This website has an open API which you can query for larger time periods, for example to train an ML model.
https://www.elexon.co.uk/documents/training-guidance/bsc-guidance-notes/bmrs-api-and-data-push-user-guide-2/

If you decide to optimise an idealised battery or gas peaking plant, this becomes like a trading problem; when do you think is best to charge and discharge, how many times should you charge and discharge the battery each day; what is the efficiency, and how much money could you make?

## Explanation of EPEX data
The EPEX price is the price of energy, £/MWh, which is auctioned at 9am the day before delivery. Typically in this auction, many renewable energy generators will sell the power they predict their wind & solar sites to generate; and suppliers (who might supply power to business and households) will purchase power according to their predicted needs. Incidentally, suppliers like Bulb purchase a lot of their power on the day ahead auction. It is an algorithmic auction, in which bids and offers are placed into the exchange for 9am, and the results are announced at 10am. The auction is pay-as-clear; ie everyone who participates gets the same price. The auction clears one price per hour: starting from 11pm (local time) the day of the auction, running until 10:30pm the next day (at which point the new day’s results will take over). 

Typically, if there is a lot of wind on the system - which has no associated fuel cost - and not much demand, for example on a Sunday when demand across the country is quite low - power prices will be cheap; especially overnight when most people are asleep and not boiling kettles! On the other hand, if there is very little wind and not much sun, and the UK has to rely on gas fired power stations for much of our energy - and gas prices are very high - power prices will be very expensive.

Each generation type (wind, solar, gas, coal, nuclear, hydro…) will have an associated run cost, depending on various economic factors such as running costs & fuel costs. At the moment, because gas prices are so high, the day ahead prices are clearing very high compared to say 1 year ago - as the cost of operating a gas fuelled power station is high.

The EPEX price is known at 10am the day before the dispatch; the spot price is known within the day, and the system price is known after the end of the half hour period in question.

## Explanation of SPOT data

The SPOT price describes the cost of power as bought and sold on the intraday market (as opposed to at day ahead). It is closer to real time, meaning that forecasts of supply and demand should be better (because, for example, weather forecasts are more accurate closer to real time). It is a closed order book, as opposed to an auction, and is typically less liquid than the day ahead market - meaning that less power is bought and sold. The SPOT price is the weighted average price from that order book and changes every half an hour. 

## Explanation of System Price

It is the job of National Grid to balance supply and demand on the UK Electricity system in real time, in order to maintain security of supply - and ensure our lights stay on! They have real time metering of the network; and if there looks to be a shortfall of energy in the next 30 minutes, they might need to phone up a power station, and ask them to increase their output. The cost of doing this across different locations and for different reasons is reflected in the ‘System Price’; which is also called the ‘System Sell Price’, ’System Buy Price’ ,’Imbalance Price’ (and all are identical). Any supplier or generator who does not do what they said they would do; ie they’ve misforecast their wind production and are suddenly have less generation (they are short) by 100MW; will have to pay for this at the system imbalance price. 
 
https://phase.modo.energy/the-energy-academy has some really nice <5min videos on these concepts we recommend you watch to gain a better understanding!

We suggest the day ahead price is the easiest to forecast; and the system price is the most challenging as it is very variable.


# Data

There are 3 csv files, containing:
- [2 years of EPEX data](epex_day_ahead_price.csv)
- [2 years of SPOT data](spot_intraday_price.csv)
- [2 years of system price data](systemprice.csv) 

For context, a settlement period (SP) is of length 30 mins and SP 1 corresponds to the period 00:00 - 00:30 in local time. There are 48 SP in a day, apart from on clock change days where there are 46 and 50.

All prices are given in £/MWh.

Assume a battery of capacity 1MWh which has available charge and discharge power of 1MW, and a round trip efficiency of 85%.

*Please note that the data belongs to Limejump and you are only allowed to use the data for the purposes of this practical exercise to be shared only with Limejump employees. Downloading the data, means that you have agreed with this usage restriction. If you do not agree with this, please do not download the data and contact us.*

# Tasks

We ask that you spend no more than 2 hours on this task. 
If you do end up spending more time on it, please indicate where you got to after 2 hours!

1.	Do some exploratory analysis on the EPEX data. Is there any seasonality in the data? What is the average spread? Are there any extreme days? Compare the other market prices: how do they correlate to one another?

2.	Derive a charge / discharge schedule for the battery to follow, optimising against EPEX price. Plot the energy and power of the battery over a few days, with price. Give an estimate of the revenues that one would expect over September 2021 with this. How does your optimisation do on the most lucrative days?

3.	How might you improve on this optimisation with more time?


