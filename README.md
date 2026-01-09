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

### Preprocessing Pipeline:

* Load Excel Sheets
* Translate Danish headers → English standardized fields
* Extract Lat/Lon from Google Maps hyperlinks
* Parse dateformat format (day.month.year)
* Clean durations and convert numeric fields
* Add computed midpoint coordinates:
MidLat = (StartLat + EndLat) / 2
MidLon = (StartLon + EndLon) / 2
* Matching Eco-Driving Violations to Trips: We assign each behavior event (eco-driving or speeding) to the trip during which it occurred by checking whether the event timestamp falls between the trip’s start and end times.
$$
\text{StartTime}_{trip} \;\le\; \text{EventTime} \;\le\; \text{EndTime}_{trip}
$$

* Matching Speeding Events to Trips:
For each speeding event, computed overspeed severity:
$$
\text{OverspeedPct} = 
\frac{\text{MaxSpeed} - \text{SpeedLimit}}{\text{SpeedLimit}} \cdot 100
$$ 
For better interpretability (how much a driver exceeded the speed limit)
* Calculated a severity penalty emphasizing dangerous overspeed: $$\text{SpeedPenaltyScore} = (\text{OverspeedPct})^{2}$$ (heavily penalizes extreme speeding)

Then aggregated per trip:
SpeedEvents:Number of speeding incidents in the trip
MaxOverspeedPct: Highest percentage over limit observed
SpeedPenalty : Sum of squared overspeed severities

* Normalization & Derived Risk Indicators
Because different trips have different lengths and contexts, the core risk indicators were normalized:
PenaltyPer10Km : Adjust harsh driving penalty based on distance traveled
SpeedPenaltyNorm : Scale speeding severity to 0–1 range
PenaltyNorm : Scaled eco-driving penalty (per distance)
ViolationsNorm : Scaled count of violations

* Calculate Trip Risk Score (0–100):
$$
\text{RiskScore}_{raw} =
0.45 \cdot \text{SpeedPenaltyNorm}
+ 0.35 \cdot \text{PenaltyNorm}
+ 0.20 \cdot \text{ViolationsNorm}
$$

We then rescaled this to a 0–100 score

* The RiskScore is calculated as a weighted combination of speeding severity, harsh driving behavior, and the frequency of violations. Speeding receives the highest weight because it has the strongest impact on accident risk, harsh driving has a moderate effect, and the violation count contributes but to a lesser degree. This weighting reflects real-world road safety research and ensures that the score emphasizes behaviors that most strongly increase driving risk.



