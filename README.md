# Pharmacy Duty Roster Scheduling Problem: A Case Study of Eskişehir

This project presents an optimization model developed in GAMS for scheduling pharmacy duty rosters in Eskişehir, Turkey. The objective is to ensure a fair and efficient distribution of duty days among pharmacies while maintaining a minimum distance of 1.5 km between any two pharmacies on duty at the same time.

## Problem Overview

The study considers 249 pharmacies scheduled over 120 time periods (e.g., days). The model minimizes the total distance between pharmacies that are on duty simultaneously, aiming for fairness and accessibility.

## Model Details

### Sets
- t: Time periods (e.g., days)
- i: Pharmacies

### Parameters
- xi, yi: Coordinates of pharmacy i
- d(i,j): Distance between pharmacy i and j, calculated using the haversine formula:

```
d(i,j) = 2 × 6370 × arcsin(√(sin²((xj−xi)/2) + cos(xi) × cos(xj) × sin²((yj−yi)/2)))
```

### Decision Variable
- s(i,t): Binary variable; 1 if pharmacy i is open in period t, 0 otherwise

## Objective Function

Minimize the total distance between pharmacies that are open at the same time:

```
z = ∑(i=1 to 249) ∑(j=1 to 249) ∑(t=1 to 120) s(i,t) × d(i,j)
```

## Constraints

- Exactly 7 pharmacies must be on duty at each time period:
  ```
  ∑i s(i,t) = 7   for all t
  ```

- Each pharmacy can be on duty at most 4 times:
  ```
  ∑t s(i,t) ≤ 4   for all i
  ```

- A pharmacy should not be on duty in consecutive or closely spaced time periods:
  ```
  s(i,t) + s(i,t+1) ≤ 1  
  s(i,t) + s(i,t+2) ≤ 1  
  s(i,t) + s(i,t+3) ≤ 1  
  s(i,t) + s(i,t+4) ≤ 1  
  ```

- For each time period and pharmacy j, at least one other pharmacy i with a distance ≥ 1.5 km must be on duty:
  ```
  For each t and j:
  ∑(i | d(i,j) ≥ 1500) s(i,t) = 1
  ```

## Summary

This model helps to optimize pharmacy duty schedules in Eskişehir by ensuring fair distribution and spatial separation of duty responsibilities. It improves access to pharmacy services and reduces workload imbalance.

---
