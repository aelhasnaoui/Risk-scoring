# Project overview:

## Develop a Driver Behavioral Risk Scoring Model that evaluates driving safety risk per trip, and can also summarize risk per driver.

### Key Outcomes:


*  A Trip Risk Score computed using real driving behavior.

* A Driver Risk Score derived from average trip risk.

* Optional mapping & clustering of geographical driving patterns.

### Data Sources:

We use four different datasets in this project:

Trips Data (Ture)
This dataset contains detailed information about each trip a vehicle makes.
It includes when the trip started and ended, where it began and ended, how far the vehicle traveled, the average and maximum speeds during the trip, and the driver who was operating the vehicle.
This dataset forms the base structure for our analysis, because each risk score is calculated per trip.

Eco Driving Data (Økokørsel)
This dataset records events where the driver demonstrates inefficient or aggressive driving behavior, such as harsh braking, rapid acceleration, and unstable driving.
These behaviors are assigned penalty points, which we use to measure how aggressive the driving style was.
This helps us understand the quality of driving, not just the route or speed.

Speeding Data (Hastighedsoverskridelser)
This dataset tracks incidents where the vehicle exceeded the speed limit.
It provides how much the driver was speeding and for how long.
These events are important because speeding is strongly linked to accident risk, so this information becomes a key factor in the risk score.

Driver Summary Sheet
This dataset summarizes the total performance of each driver over a selected period.
It includes total distance driven, average speed, number of trips, and total penalties.
We use this summary to compute a driver-level risk score, based on the average behavior observed across all trips.

# Project overview:

## Develop a Driver Behavioral Risk Scoring Model that evaluates driving safety risk per trip, and can also summarize risk per driver.

### Key Outcomes:


*  A Trip Risk Score computed using real driving behavior.

* A Driver Risk Score derived from average trip risk.

* Optional mapping & clustering of geographical driving patterns.

### Data Sources:

We use four different datasets in this project:

Trips Data (Ture)
This dataset contains detailed information about each trip a vehicle makes.
It includes when the trip started and ended, where it began and ended, how far the vehicle traveled, the average and maximum speeds during the trip, and the driver who was operating the vehicle.
This dataset forms the base structure for our analysis, because each risk score is calculated per trip.

Eco Driving Data (Økokørsel)
This dataset records events where the driver demonstrates inefficient or aggressive driving behavior, such as harsh braking, rapid acceleration, and unstable driving.
These behaviors are assigned penalty points, which we use to measure how aggressive the driving style was.
This helps us understand the quality of driving, not just the route or speed.

Speeding Data (Hastighedsoverskridelser)
This dataset tracks incidents where the vehicle exceeded the speed limit.
It provides how much the driver was speeding and for how long.
These events are important because speeding is strongly linked to accident risk, so this information becomes a key factor in the risk score.

Driver Summary Sheet
This dataset summarizes the total performance of each driver over a selected period.
It includes total distance driven, average speed, number of trips, and total penalties.
We use this summary to compute a driver-level risk score, based on the average behavior observed across all trips.


