# Scaling Policies

| Policy             | Trigger                 | Reaction Speed | Complexity | Best For                      |
| ------------------ | ----------------------- | -------------- | ---------- | ----------------------------- |
| Target Tracking    | Metric target value     | Fast           | Low        | Most general workloads        |
| Step Scaling       | Alarm thresholds        | Fast           | Medium     | Fine-grained metric control   |
| Simple Scaling     | Single CloudWatch alarm | Slow           | Low        | Legacy or simple setups       |
| Scheduled Scaling  | Time / cron expression  | Proactive      | Low        | Predictable traffic patterns  |
| Predictive Scaling | ML-based forecast       | Proactive      | Medium     | Recurring/historical patterns |
## 1. Target Tracking Scaling

The **simplest and most recommended** policy. You pick a metric and a target value — ASG automatically adds or removes instances to maintain that target, similar to how a thermostat works.

### How It Works

- ASG creates CloudWatch alarms automatically behind the scenes
- Scales out aggressively, scales in conservatively (built-in stabilization)
- You don't define specific instance counts — ASG calculates the required capacity

### Supported Metrics

|Metric|Description|
|---|---|
|`ASGAverageCPUUtilization`|Average CPU across all instances|
|`ASGAverageNetworkIn/Out`|Average network bytes in/out|
|`ALBRequestCountPerTarget`|Requests per target in an ALB target group|

> You can also define **custom CloudWatch metrics** (e.g., memory usage, SQS queue depth).

### Example

> Target CPU = **50%**
> 
> - CPU rises to 80% → ASG **adds** instances
> - CPU drops to 20% → ASG **removes** instances

### Notes

- Scale-in cooldown defaults to **300 seconds** to avoid thrashing
- Best for most general-purpose workloads

---

## 2. Step Scaling

More **granular control** than target tracking. You define CloudWatch alarms and specify how many instances to add or remove at each threshold step.

### How It Works

- You manually create a CloudWatch alarm
- Define "steps" — ranges of metric values mapped to scaling adjustments
- Reacts faster than simple scaling because it **does not wait** for a cooldown between steps

### Example — Scale Out

|CPU Range|Action|
|---|---|
|60% – 75%|Add 1 instance|
|75% – 90%|Add 3 instances|
|> 90%|Add 5 instances|

### Example — Scale In

|CPU Range|Action|
|---|---|
|30% – 40%|Remove 1 instance|
|< 30%|Remove 2 instances|

### Adjustment Types

|Type|Behavior|
|---|---|
|`ChangeInCapacity`|Add/remove a fixed number (e.g., +2)|
|`ExactCapacity`|Set desired to a specific number (e.g., = 6)|
|`PercentChangeInCapacity`|Add/remove by percentage (e.g., +30%)|

---

## 3. Simple Scaling

The **oldest and most basic** policy. One alarm triggers one action, then ASG waits for a cooldown before doing anything else.

### How It Works

1. CloudWatch alarm breaches a threshold
2. ASG executes the scaling action (add/remove N instances)
3. ASG waits for the **cooldown period** (default 300s)
4. All other alarms are **ignored** during cooldown

### Limitation

The mandatory cooldown makes it slow to respond to rapid or sustained changes. Step Scaling and Target Tracking are preferred over Simple Scaling in modern setups.

---

## 4. Scheduled Scaling

Scale based on **time**, not metrics. You define a schedule and ASG adjusts min/max/desired capacity at that point in time.

### Use Cases

- Business hours traffic (more capacity 9 AM – 6 PM)
- Weekly or nightly batch jobs
- Pre-warming before a known event (product launch, flash sale)

### Configuration

- Supports one-time actions or **recurring schedules** via cron expressions
- You can set the desired, min, and/or max capacity for the scheduled time

### Example

```
# Scale up on weekday mornings
Cron: 0 8 * * MON-FRI  →  desired = 10, min = 5

# Scale down on weekday evenings
Cron: 0 20 * * MON-FRI →  desired = 2,  min = 1
```

---

## 5. Predictive Scaling

Uses **machine learning** to forecast future demand and proactively provisions capacity before it's needed — rather than reacting after the fact.

### How It Works

- Analyzes up to **14 days** of CloudWatch historical data
- Generates a **48-hour forecast**, refreshed every 24 hours
- Launches instances **up to 1 hour before** a predicted spike

### Modes

|Mode|Behavior|
|---|---|
|**Forecast Only**|Generates predictions without scaling — good for evaluating accuracy before enabling|
|**Forecast and Scale**|Actively adjusts capacity based on the ML forecast|

> Best paired with **Target Tracking** to handle unexpected deviations from the forecast.

---
## Cooldown vs. Warm-up

These two concepts are often confused:

|Concept|Applies To|Description|
|---|---|---|
|**Cooldown**|Simple Scaling|A pause _after_ a scaling action before the next one can trigger. Prevents over-scaling.|
|**Instance Warm-up**|Step & Target Tracking|Time a new instance needs to be ready. ASG excludes it from metric calculations during this period to avoid premature further scaling.|

---

## Recommended Production Setup

For most production workloads, a **layered approach** works best:

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  Predictive Scaling   →  handles forecasted load    │
│         +                                           │
│  Target Tracking      →  handles real-time load     │
│         +                                           │
│  Scheduled Scaling    →  handles known events       │
│                                                     │
└─────────────────────────────────────────────────────┘
```

This combination covers **proactive**, **reactive**, and **planned** scaling scenarios, giving your ASG the best chance of maintaining performance and cost efficiency.

---

## Key Limits to Keep in Mind

- **Minimum capacity**: ASG will never scale below this value
- **Maximum capacity**: ASG will never scale above this value
- **Desired capacity**: the current target number of running instances
- Scaling policies operate **within** the min/max bounds at all times