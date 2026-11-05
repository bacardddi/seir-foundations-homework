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

The server showed 22:42 UTC when my own clock said 5:42 PM Eastern. That means the times were 5 hours apart. Daylight saving time ended on Nov 1, so Eastern time is now UTC−5. Servers use UTC so logs from different time zones all line up, and the time doesn't jump when daylight saving changes.

## Script 4 — hw4_disk_watch.py

Slack message:
```
Severity: INFO
Disk check on / : 25.4% used, 5.9 GB free. Severity chosen by MY code.
```

**Question: why is a quiet INFO here GOOD design, not a failure?**

My disk was only 25.4% full, which is healthy. So the script correctly sent an INFO alert instead of WARNING or CRITICAL. A good monitor only gets loud when something is actually wrong. If every check sent a big alert, people would start ignoring them — that's called alert fatigue. A quiet INFO just shows the check worked and everything is fine.

## Script 5 — hw5_load_check.py

Slack message:
```
Severity: INFO
Load average: 0.00 (1m), 0.00 (5m), 0.00 (15m) on 1 CPU(s).
```

**Question: what would make this number spike on the Balerica bank?**

My load average was 0.00 because nothing was running. It would spike if lots of customers hit the API at the same time (like payday), if a big COBOL batch job was running, or if something got stuck in a loop. Since the server only has 1 CPU, a load above 1.0 means jobs are waiting.

## Script 6 — hw6_incident_drill.py

Slack message:
```
Severity: ERROR
Incident: DRILL-3510
(DRILL — not real) FastAPI /payments endpoint responded 500 twice in one minute.
```

**Question: which of these fake incidents deserves to wake a human at 3 AM?**

The /payments endpoint returning 500 errors should wake someone, because customers can't make payments and the bank is losing money and trust. A batch job running 12 minutes long is just a WARNING and can wait until morning. Only problems that hurt customers or money right now should wake a human at night.

## Script 7 — hw7_morning_report.py

Screenshot: `screenshots/hw7_morning_report.jpg`

---

## Experiment 1 — SLACK_ALERT_SEVERITY=CRITICAL, rerun script 2

**My prediction (before running):** With CRITICAL as the threshold, only the CRITICAL alert should get through.

**Actual result:**
```
$ grep SEVERITY .env
SLACK_ALERT_SEVERITY=CRITICAL
$ python hw2_severity_tour.py
INFO     -> skipped by your threshold
WARNING  -> skipped by your threshold
ERROR    -> skipped by your threshold
CRITICAL -> DELIVERED to Slack
```
My prediction was right: only 1 of the 4 messages arrived, the CRITICAL one. CRITICAL is the highest level (3), so INFO, WARNING and ERROR were all below the threshold and skipped.

## Experiment 2 — SLACK_ALERTS_ENABLED=false, run script 1

```
$ grep SLACK_ALERT .env
SLACK_ALERTS_ENABLED=false
SLACK_ALERT_SEVERITY=INFO
SLACK_ALERT_CHANNEL=balerica-alerts
$ python hw2_severity_tour.py
INFO     -> skipped by your threshold
WARNING  -> skipped by your threshold
ERROR    -> skipped by your threshold
CRITICAL -> skipped by your threshold
```

**Why a kill switch is useful during maintenance windows:**

When `SLACK_ALERTS_ENABLED=false`, nothing gets sent to Slack, not even CRITICAL alerts. The script still runs, but `send_alert()` stops right away. This is helpful during a maintenance window, which is a planned time when engineers restart servers, install updates, or move databases. During maintenance, things are supposed to look broken for a few minutes, so without a kill switch the Slack channel would fill with false alarms and maybe wake people up for no reason.

Turning alerts off with one setting in `.env` is safer than editing or deleting code. When maintenance is done, you just set the value back to true, and monitoring starts again without touching any scripts.

## Experiment 3 — lower the WARNING threshold in hw4_disk_watch.py

My disk was 25.4% used, so I changed the WARNING line from 75 to 20 in nano:
```
$ grep -n "percent_used >=" hw4_disk_watch.py
16:if percent_used >= 90:
18:elif percent_used >= 20:
$ python hw4_disk_watch.py
Disk is 25.4% used -> severity WARNING.
Delivered!
```

**Did the severity change?** TODO

## Experiment 4 — put everything back

I changed the disk WARNING line back to 75 and checked that alerts are on with the INFO threshold. Then I reran script 4, and it was back to a quiet INFO:
```
$ grep -n "percent_used >=" hw4_disk_watch.py
16:if percent_used >= 90:
18:elif percent_used >= 75:
$ grep SLACK_ALERT .env
SLACK_ALERTS_ENABLED=true
SLACK_ALERT_SEVERITY=INFO
SLACK_ALERT_CHANNEL=balerica-alerts
$ python hw4_disk_watch.py
Disk is 25.4% used -> severity INFO.
Delivered!
```
