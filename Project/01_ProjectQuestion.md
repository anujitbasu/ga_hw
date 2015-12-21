#Project Introduction

## Predicting Taxi Ride Destinations

Anujit Basu

**What is the question you hope to answer?**

Given some attributes of an in-progress taxi ride, and a polyline of its current path, is it possible to predict the destination of the taxi ride?

**Why did you select this question?**

I could not get data directly connected to my workplace for a class project. And since I am interested in a general exposure to 
Machine Learning algorithms, I decided to looks for a problem and dataset of a general interest. While perusing various open 
and closed competitions on Kaggle, I stumbled upon this closed competition. This problem has a couple of aspects I find interesting. It has a time-series like component in the polyline of the ongoing taxi-ride's path. It also deals with a lot of geographical data. These are aspects that are of interest to me from earlier times of my career.

The city of Porto has a taxi system where people can request taxi rides by contacting a central dispatch. This central dispatch / system would be able to provide a better sceduling service if it could predict the availability of taxis in the callers' viscinity in the near future. In order to achive this, it would be helpful to determine if any of the currently in-progress rides will terminate in the area of interest in the time-frame of interest. In other words, knowing details of an in-progress ride, it would be helpful to predict the destination of the ride and the time of arrival at the destination.

My project is the first of these two questions. There is actually a follow-up competition on Kaggle addressing the second question (predict ride termination time).

**Data**

The Kaggle competition provides data for every taxi ride from every taxi in the Porto system over year. This forms the training dataset and contains upwards of 1.7 million records.

There is a much smaller test dataset that was used to evaluate the competition submissions. Since the competition is now closed, this test dataset cannot be used. I will need to reserve some of the training dataset records for testing purposes.

The ride data consist of the following attributes (columns):

  * CALL_TYPE : An indicator for if the ride was requested from the central dispatch, if requested at a taxi stand, or hailed at a random location.
  * ORIGIN_CALL : A telephone ID for calls requested fron central dispatch
  * ORIGIN_STAND : A taxi stand ID for calls originating at a taxi stand. There is a seperate file provided with taxi stand details.
  * TAXI_ID : An ID for the taxi.
  * TIMESTAMP : A unix timestamp for the start of the ride.
  * DAY_TYPE : An ID for a weekday vs a weekend vs a holiday
  * MISSING_DATA : An indnicator to alert that some data might be missing
  * POLYLINE : A series of [Longitude, Latitude] points signifying the path of the ride. The first point is the start location and the last point is the destination.


