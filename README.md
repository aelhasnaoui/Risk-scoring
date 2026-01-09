# Project Overview

## Goal
Develop a **Driver Behavioral Risk Scoring Model** that evaluates **driving safety risk per trip** and can also **summarize risk per driver**.

## Key Outcomes
- **Trip Risk Score** computed using real driving behavior  
- **Driver Risk Score** derived from average trip risk  
- Optional **mapping & clustering** of geographical driving patterns  

---

## Data Sources
We use four datasets:

### 1) Trips Data *(Ture)*
Contains detailed information about each trip a vehicle makes, including:
- trip start/end time  
- start/end location  
- distance traveled  
- average and maximum speed  
- driver ID  

This dataset forms the base structure of the analysis because **each risk score is calculated per trip**.

### 2) Eco Driving Data *(Økokørsel)*
Records events that reflect inefficient or aggressive driving behavior, such as:
- harsh braking  
- rapid acceleration  
- unstable driving  

These behaviors are assigned **penalty points**, which we use to measure driving aggressiveness.  
This helps capture **driving quality**, not just route or speed.

### 3) Speeding Data *(Hastighedsoverskridelser)*
Tracks incidents where the vehicle exceeded the speed limit, including:
- how much the driver was speeding  
- for how long  

Speeding is strongly linked to accident risk, so it becomes a **key factor** in the risk score.

### 4) Driver Summary Sheet
Summarizes driver performance over a selected period, including:
- total distance driven  
- average speed  
- number of trips  
- total penalties  

We use this summary to compute a **driver-level risk score** based on average behavior across trips.

---

## Preprocessing Pipeline

### Data Loading & Standardization
- Load Excel sheets  
- Translate Danish headers → English standardized fields  
- Extract latitude/longitude from Google Maps hyperlinks  
- Parse date format: `day.month.year`  
- Clean durations and convert numeric fields  

### Derived Coordinates
Compute midpoint coordinates for each trip:
- `MidLat = (StartLat + EndLat) / 2`  
- `MidLon = (StartLon + EndLon) / 2`  

---

## Matching Events to Trips

### Matching Eco-Driving Violations to Trips
Each behavior event (eco-driving or speeding) is assigned to the trip during which it occurred by checking whether the event timestamp falls between the trip start and end times:

$$
\text{StartTime}_{trip} \le \text{EventTime} \le \text{EndTime}_{trip}
$$

### Matching Speeding Events to Trips
For each speeding event, compute overspeed severity:

$$
\text{OverspeedPct} =
\frac{\text{MaxSpeed} - \text{SpeedLimit}}{\text{SpeedLimit}} \cdot 100
$$

For better interpretability (how much a driver exceeded the speed limit), we also compute a severity penalty that emphasizes dangerous overspeed:

$$
\text{SpeedPenaltyScore} = (\text{OverspeedPct})^{2}
$$

This heavily penalizes extreme speeding.

### Per-Trip Speeding Aggregation
After matching speeding events to trips, we aggregate:

- **SpeedEvents**: number of speeding incidents in the trip  
- **MaxOverspeedPct**: highest percentage over limit observed  
- **SpeedPenalty**: sum of squared overspeed severities  

---

## Normalization & Derived Risk Indicators
Because trips vary in length and context, core indicators are normalized:

- **PenaltyPer10Km**: eco-driving penalty adjusted by distance traveled  
- **SpeedPenaltyNorm**: speeding severity scaled to a 0–1 range  
- **PenaltyNorm**: scaled eco-driving penalty (distance-adjusted)  
- **ViolationsNorm**: scaled count of violations  

---

## Trip Risk Score (0–100)

### Weighted Risk Formula

Trip risk is computed as a weighted combination of speeding severity, harsh driving behavior, and frequency of violations:

$$
\text{RiskScore}_{raw} =
0.45 \cdot \text{SpeedPenaltyNorm}
+ 0.35 \cdot \text{PenaltyNorm}
+ 0.20 \cdot \text{ViolationsNorm}
$$

We then rescale `RiskScore_raw` to a **0–100** score.

### Rationale for Weighting
- **Speeding** receives the highest weight because it has the strongest association with accident risk.  
- **Harsh driving** has a moderate effect and captures aggressive behavior.  
- **Violation frequency** contributes as a general risk intensity signal but at a lower weight.  

This weighting emphasizes behaviors most strongly linked to elevated driving risk.
