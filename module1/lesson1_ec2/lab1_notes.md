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
- TODO

## Run 5 — i-TODO (date TODO)

Exact output line from this run:
```
TODO
```

Notes:
- TODO

## Run 6 — i-TODO (date TODO)

Exact output line from this run:
```
TODO
```

Notes:
- TODO

## Run 7 — i-TODO (date TODO)

Exact output line from this run:
```
TODO
```

Notes:
- TODO

---

## COBOL practice

Run used: Run 4 — Instance ID i-07a13735074905900

### Accounts (`cat data/accounts.dat`)
Number of accounts: TODO
Account IDs: TODO

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
TODO

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
TODO

---

## BAM (extra credit)

### BAM_2.1 — Bedrock
Instance ID: TODO
IAM role name: TODO
Inference profile ID: TODO
Question asked + full response:
```
TODO
```

### BAM_2.2 — Baked launch template
Instance ID: TODO
Launch template ID + version: TODO
Final User Data script:
```bash
TODO
```
What I had to fix: TODO
Why User Data runs as root by default: TODO

### BAM_2.3 — Golden AMI
AMI ID: TODO
Snapshot ID: TODO
Built from instance: TODO
Launched from AMI: TODO
Were my earlier test payments still in `data/payments.dat`? Why? TODO
Launch template vs AMI — when to use each: TODO

---

## Mistakes I made
1. TODO
2. TODO
3. TODO

## Commands I now know by heart
- TODO
