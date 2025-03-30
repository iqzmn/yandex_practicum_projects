# Project Objective

The popular scooter rental service GoFast allows customers to make trips around the city using a mobile application. The following parameters should be analyzed to help business development:
* Service popularity in cities
* Ratio of users with and without subscription
* User age
* Distance per trip
* Trip duration

We will test the following hypotheses:
* Do subscription users spend more time on trips?
* The average distance traveled by subscription users per trip does not exceed 3130 meters.
* Will the monthly revenue from subscription users be higher than revenue from non-subscription users.

# Data Description

We received data about users, their trips, and subscriptions contained in CSV files.

***Users*** — users_go.csv
* 'user_id' - unique user identifier
* 'name' - username
* 'age' - age
* 'city' - city
* 'subscription_type' - subscription type (free, ultra)

***Trips*** — rides_go.csv
* 'user_id' - unique user identifier
* 'distance' - distance traveled by the user in the current session (in meters)
* 'duration' - session duration (in minutes) — time from when the user pressed the "Start Trip" button until they pressed the "End Trip" button
* 'date' - date of the trip

***Subscriptions*** — subscriptions_go.csv
* 'subscription_type' - subscription type
* 'minute_price' - cost of one minute of travel with this subscription
* 'start_ride_price' - cost to start a trip
* 'subscription_fee' - cost of monthly payment

The service can be used:
* without subscription
   * no monthly fee
   * cost of one minute of travel — 8 rubles
   * cost of starting a trip — 50 rubles
* with Ultra subscription
   * monthly fee — 199 rubles per month
   * cost of one minute of travel — 6 rubles
   * cost of starting a trip — free

# Research Stages
1. Data loading
2. Data preprocessing
3. Exploratory data analysis
4. Data merging
5. Data calculation
6. Hypothesis testing
7. General conclusion

# Research Conclusion

Thus, we can conclude that the GoFast service has potential for growth and development in the scooter rental market, but it needs to consider the specifics of different cities and target audiences, as well as encourage users to switch to the Ultra subscription. The hypothesis that subscription users spend more time per session in the service was confirmed. The hypothesis that the average distance traveled by subscription users per trip does not exceed the optimal 3130 meters was confirmed. The hypothesis that monthly revenue from subscription users is higher than revenue from non-subscription users was confirmed.
