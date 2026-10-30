1. Create Security Group
Allow these ports:

22 → SSH

80 → Web browser access

8000 → Bank API access

These rules let you log in, open the site, and reach the API.

2. Create Instance Template
Include:

AMI

Instance type

Security group

Key pair

User‑data script

This saves your setup so you can launch identical servers fast

3. Add user‑data.sh
This script runs automatically when the instance boots.

4. Launch Instance From Template
Go to:

EC2/ Instance Templates/ Actions/ Launch Instance

Wait until the instance shows running.

5. Get the Public DNS
Copy the instance’s Public DNS — you’ll need it for:

Browser testing

SSH login

API access

6. SSH Into the Instance

7. Verify You’re Inside the Server

8. Install Git

9. Clone the Course Repo

10. Run the Installer

11. Run COBOL Core

12. Start the API
Leave this window running.

13. Open the API in the Browser
This loads the bank’s API.


