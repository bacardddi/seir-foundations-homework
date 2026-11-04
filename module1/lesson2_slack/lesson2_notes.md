# Lesson 2 — Notes (Bakari Woods)

## Script 1 — hw1_hello_slack.py

Output:
```
Delivered! Go look at #balerica-alerts. That message is yours.
```

## Script 2 — hw2_severity_tour.py

Output:
```
INFO     -> DELIVERED to Slack
WARNING  -> DELIVERED to Slack
ERROR    -> DELIVERED to Slack
CRITICAL -> DELIVERED to Slack
```

**Question: which severities arrived, and WHY those?**

All four alert levels showed up: INFO, WARNING, ERROR, and CRITICAL. This happened because my .env file has `SLACK_ALERT_SEVERITY=INFO`. INFO is the lowest level. In slack.py, the levels are ranked like this:

- INFO = 0
- WARNING = 1
- ERROR = 2
- CRITICAL = 3

The script only skips alerts that are below the threshold. Since nothing is below INFO, everything went through. If the threshold were ERROR, which is the default in the class, then only ERROR and CRITICAL alerts would show up.

Later my threshold was set to ERROR and I ran script 2 again, which proved it:
```
INFO     -> skipped by your threshold
WARNING  -> skipped by your threshold
ERROR    -> DELIVERED to Slack
CRITICAL -> DELIVERED to Slack
```

## Script 3 — hw3_server_time.py

Slack message:
```
Time check from EC2: it is Wednesday November 04, 2026 at 22:42:44 UTC here. Servers speak UTC!
```

**Question: how many hours apart are server time and YOUR wall clock?**

TODO

## Script 4 — hw4_disk_watch.py

Slack message:
```
Severity: INFO
Disk check on / : 25.4% used, 5.9 GB free. Severity chosen by MY code.
```

**Question: why is a quiet INFO here GOOD design, not a failure?**

TODO

## Script 5 — hw5_load_check.py

Slack message:
```
Severity: INFO
Load average: 0.00 (1m), 0.00 (5m), 0.00 (15m) on 1 CPU(s).
```

**Question: what would make this number spike on the Balerica bank?**

TODO

## Script 6 — hw6_incident_drill.py

Slack message:
```
Severity: ERROR
Incident: DRILL-3510
(DRILL — not real) FastAPI /payments endpoint responded 500 twice in one minute.
```

**Question: which of these fake incidents deserves to wake a human at 3 AM?**

TODO

## Script 7 — hw7_morning_report.py

Screenshot: `screenshots/hw7_morning_report.jpg`

---

## Experiment 1 — SLACK_ALERT_SEVERITY=CRITICAL, rerun script 2

**My prediction (before running):** With CRITICAL as the threshold, only the CRITICAL alert should get through.

**Actual result:** TODO

## Experiment 2 — SLACK_ALERTS_ENABLED=false, run script 1

TODO

## Experiment 3 — lower the WARNING threshold in hw4_disk_watch.py

TODO

## Experiment 4 — put everything back

TODO
