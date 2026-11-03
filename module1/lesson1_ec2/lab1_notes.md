# Lesson 1 — Lab Notes (Bakari Woods)

<!-- For each run: at least 3 sentences in your own words.
     1) What went differently this time?
     2) What error (if any) did you hit, and what fixed it?
     3) What did you understand this time that you did not before? -->

## Run 1 — i-0dc9309767bd71db5 (2026-11-01)

Exact output line from this run:
```
INFO:     Started server process [12035]
```

Notes:
This was my first solo run after class, so I took my time and followed the runbook step‑by‑step. Everything went smoothly — no errors at all — although the installer ran for several minutes and printed a lot of pip output, which made me wonder at first if it had frozen. I learned that running `account_lookup 100001` returns Alice Johnson’s account with a balance of 5000.00, and seeing `echo $?` return 0 confirms the COBOL program executed successfully.

## Run 2 — i-06aef3f6d3b08c30e (2026-11-01)

Exact output line from this run:
```
INFO:     Started server process [12327]
```

Notes:
For this run, I launched two instances from my template at the same time, which is why Runs 2 and 3 show identical boot timestamps. I noticed my terminal prompt switched to root instead of ec2-user, meaning I had elevated myself to the root account even though the assignment requires staying under ec2-user. This time I also understood that running `source venv/bin/activate` is what adds (venv) to the prompt, and that Uvicorn won’t start unless the virtual environment is activated first.

## Run 3 — i-080c1288140029da3 (2026-11-01)

Exact output line from this run:
```
INFO:     Started server process [35681]
```

Notes:
This run went quicker because the second instance was already up, so I didn’t have to wait for it to finish booting. Everything worked without errors, and I realized that running the instance‑ID command before starting Uvicorn is a reliable way to confirm which server I’m actually on — the ID printed in the terminal matches the one shown at the bottom of the EC2 console. I also now understand that using `--host 0.0.0.0` allows the API to accept external connections, not just requests coming from inside the instance itself.

## Run 4 — i-07a13735074905900 (2026-11-02)

Exact output line from this run:
```
INFO:     Started server process [54804]
```

Notes:
This run was a little different because I started the instance on Sunday night and kept it running past midnight, so the screenshot ended up dated Monday, Nov 2. The boot time still shows Nov 1, which is fine since the requirement only says the boot time must be within 24 hours of the screenshot. Uvicorn started without any issues, but I discovered that my first ledger screenshot didn’t count because it came from an instance that wasn’t listed in my run table. I also learned that I need to run the instance‑ID command before every screenshot, not just the one before launching Uvicorn.

This time the COBOL return codes made a lot more sense to me:

- **0** — the program ran successfully
- **4** — the account number doesn’t exist
- **8** — a bank rule blocked the payment
- **12** — the program couldn’t locate its data file because I executed it from the wrong directory, which produced `FILE-STATUS=35`

## Run 5 — i-0a90d322749389b41 (2026-11-02)

Exact output line from this run:
```
INFO:     Started server process [12025]
```

Notes:
For this run, I launched two instances from my template at the same time, which is why Runs 5 and 6 share the exact same boot timestamp (06:04:23 UTC, about 1:04 AM Eastern). Since I’d already completed four runs, I was able to type most of the commands from memory and only glanced at the runbook a couple of times. Everything executed without errors, but I noticed that the installer upgrades pip from 21.3.1 to 26.0.1 every single run. That happens because each instance starts out completely clean, with nothing preinstalled. Seeing that helped me understand why the assignment requires brand‑new instances each time — every run demonstrates that I can rebuild the entire bank system from scratch.

## Run 6 — i-0577b13eec0179ee4 (2026-11-02)

Exact output line from this run:
```
INFO:     Started server process [12263]
```

Notes:
This was my final run for Monday, so I made sure it served as the third and last lab‑run screenshot dated Nov 2, keeping me under the daily limit of three. I had already installed everything on the second instance while Run 5 was finishing, so I jumped straight to Step 4. Again, there were no errors, but my prompt still showed root instead of ec2-user, which I need to fix before Run 7. I also learned that each “Started server process” number (this time 12263) is a unique process ID, which proves that every run is a separate server instance.

## Run 7 — i-0541cfa5be7ef05c4 (2026-11-03)

Exact output line from this run:
```
INFO:     Started server process [12395]
```

Notes:
On my last run, I finally felt like I understood the whole process. I did it just after midnight on Tuesday, so it became my only lab‑run screenshot for Nov 3. Everything worked with no errors. The commands are getting easier to do and understand — installing Git, cloning the repo, running the installer, moving into /opt/balerica-bank, activating the venv, running the ID line, and starting Uvicorn.

But even though I meant to fix it, my prompt still showed root, which reminded me that I need to slow down at Step 0 and check for # vs $ before I start. That’s a habit I’m still learning.

---

## COBOL practice

Run used: Run 4 — Instance ID i-07a13735074905900

### Accounts (`cat data/accounts.dat`)
Number of accounts: 3
Account IDs: 100001 (Alice Johnson), 100002 (Robert Smith), 100003 (Maria Garcia) — all ACTIVE, all USD

### Return code 0 — success
Command: `./cobol/bin/account_lookup 100001`
Output:
```
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
$ echo $?
0
```
Meaning: 0 = success. The account was found and its details were printed.

### Return code 4 — account not found
Command: `./cobol/bin/account_lookup 999999`
Output:
```
ERROR=ACCOUNT NOT FOUND
ACCOUNT=999999
$ echo $?
4
```
Meaning: 4 = account not found. The program ran fine but there is no account 999999 in the ledger, and it did not crash.

### Return code 8 — business rejection
Command: `./cobol/bin/payment_post TXN-BW-0001 100001 0.00 USD TEST-ZERO`
Output:
```
TXN=TXN-BW-0001
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO
$ echo $?
8
```
Command (amount bigger than balance): `./cobol/bin/payment_post TXN-BW-0002 100001 99999999.00 USD TEST-BIG`
Output:
```
TXN=TXN-BW-0002
ACCOUNT=100001
AMOUNT=99999999.00
CURRENCY=USD
STATUS=REJECTED
REASON=INSUFFICIENT FUNDS
$ echo $?
8
```
Meaning: 8 = business rejection. The request was understood, but a bank rule said no (zero amount, or not enough money in the account).

### Return code 12 — environment failure
Command: `cd ~` then `/opt/balerica-bank/cobol/bin/account_lookup 100001`
Output:
```
ERROR=CANNOT OPEN data/accounts.dat
FILE-STATUS=35
$ echo $?
12
```
Meaning: 12 = environment failure. I ran it from the wrong folder, so the program could not find `data/accounts.dat` (it uses a relative path). FILE-STATUS 35 is COBOL's "file not found" code.

### Bonus — payment to a non-ACTIVE account
All three accounts in `data/accounts.dat` were ACTIVE, so there was no inactive account to test.

### Decoding Alice's balance by hand
Raw 12 characters (columns 45–56): `000000500000`
Dollar amount: $5,000.00
Why there is no decimal point in the file:

- **The final two digits represent cents:** A number like `000000500000` is interpreted as $5,000.00, because the last two positions are always the cents portion.
- **It was designed to conserve memory:** COBOL used this format because computer storage was extremely costly in 1959, so every saved byte mattered.
- **The field always has a fixed length:** Since the width never changes, programs like `cut` can reliably extract the value based on its exact position in the record.

### My journal lines (`cat data/payments.dat`)
```
TXN-BW-0001         1000010000000000000USDREJECTEDTEST-ZERO
TXN-BW-0002         1000010099999999900USDREJECTEDTEST-BIG
TXN-BW-0001         1000010000000000000USDREJECTEDTEST-ZERO
TXN-BW-0002         1000010099999999900USDREJECTEDTEST-BIG
```
Lookup of 100001 afterwards:
```
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
```
Did account 100001's balance change after my payments? No — it is still 5000.00.
My best guess why:

- **Both transactions failed:** One attempt was for $0, and the other exceeded Alice’s available balance. Since rejected transactions never actually transfer funds, her account balance didn’t change.
- **Every attempt still gets logged:** Even when a payment is denied, the program records it in the journal. This ensures the bank has a complete history of all activity, which is essential for proper auditing.
- **The same transaction ID appeared twice:** In a real banking system, duplicate TXN IDs would normally be blocked to prevent confusion or accidental double‑processing.

### Optional — SQLite tables
Not attempted (optional).

---

## BAM (extra credit)

Not attempted this week (optional).

---

## Mistakes I made
1. **Working as root instead of ec2‑user** — My prompt showed root and `whoami` also said root in some screenshots, but the assignment needs the ID line to show ec2‑user. I learned I should always check my prompt before I start, and type `exit` if I accidentally end up in a root shell.
2. **Taking a ledger screenshot on the wrong instance** — My first ledger screenshot came from an instance that wasn’t in my run table, and I had already used my three allowed runs for Nov 1. I fixed it by keeping the instance running past midnight so it counted as Run 4 on Nov 2, then I retook the screenshot correctly.
3. **Forgetting the ID line on my return‑code screenshots** — My first COBOL return‑code screenshots (rc0, rc4, rc8, rc12) didn’t include the required ID line: `whoami`, `date`, `ec2-metadata -i`, and `uptime -s`. I redid them the right way, and now I run the ID line before every screenshot so I don’t miss it again.
4. **Using the same transaction IDs twice** — I reused TXN‑BW‑0001 and TXN‑BW‑0002, so they show up twice in `payments.dat`. Real banks don’t allow duplicate transaction IDs because it can cause mistakes, so I should have used TXN‑BW‑0003 and higher for the second attempts.

## Commands I now know by heart
- `sudo dnf install -y git` — this installs Git on a brand‑new Amazon Linux server so I can use it right away.
- `git clone https://github.com/BalericaAI/SEIR-Foundations.git` — this downloads the whole course repo onto my instance, including all the folders and files I need.
- `bash SEIR-Foundations/module1/lesson1_ec2/API/install/install.sh` — this runs the installer script that sets up the bank system, installs everything, and gets the COBOL programs ready.
- `cat data/accounts.dat` / `cat data/payments.dat` — these commands show me the ledger and the payments journal so I can see the account balances and every payment attempt.
- `cut -c45-56 data/accounts.dat` — this pulls out just the balance column by using the exact character positions in the file.
