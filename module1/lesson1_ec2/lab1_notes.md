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
- TODO
- TODO
- TODO

## Run 2 — i-06aef3f6d3b08c30e (2026-11-01)

Exact output line from this run:
```
INFO:     Started server process [12327]
```

Notes:
- TODO
- TODO
- TODO

## Run 3 — i-080c1288140029da3 (2026-11-01)

Exact output line from this run:
```
INFO:     Started server process [35681]
```

Notes:
- TODO
- TODO
- TODO

## Run 4 — i-TODO (date TODO)

Exact output line from this run:
```
TODO
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

Run used: Run TODO — Instance ID i-TODO

### Accounts (`cat data/accounts.dat`)
Number of accounts: TODO
Account IDs: TODO

### Return code 0 — success
Command: `./cobol/bin/account_lookup 100001`
Output:
```
TODO
```
Meaning: TODO

### Return code 4 — account not found
Command: `./cobol/bin/account_lookup 999999`
Output:
```
TODO
```
Meaning: TODO

### Return code 8 — business rejection
Command: `./cobol/bin/payment_post TXN-BW-0001 100001 0.00 USD TEST-ZERO`
Output:
```
TODO
```
Command (amount bigger than balance): `TODO`
Output:
```
TODO
```
Meaning: TODO

### Return code 12 — environment failure
Command: `cd ~` then `/opt/balerica-bank/cobol/bin/account_lookup 100001`
Output:
```
TODO
```
Meaning: TODO

### Bonus — payment to a non-ACTIVE account
TODO

### Decoding Alice's balance by hand
Raw 12 characters (columns 45–56): `TODO`
Dollar amount: TODO
Why there is no decimal point in the file: TODO

### My journal lines (`cat data/payments.dat`)
```
TODO
```
Did account 100001's balance change after my payments? TODO
My best guess why: TODO

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
