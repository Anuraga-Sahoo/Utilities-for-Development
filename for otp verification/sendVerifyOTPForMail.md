# How to generate otp using nodeMailer

### Step 1
First you need to open a google account and then complete the two step verification it is necessary

### Step 2
After that search "Google Accounts" then click on "Go to Google Account"

### Step 3
Check your accounts 2 step verification is on
In search bar search "app Password"

### Step 4
Then google ask your gmail password. simply enter the correct password and then you Enter the app name and click on the create button . after clicking on create button it gives you your app password copy this and keep it save 

### Step 5
create a .env file and paste that on .env like this and remove the blank spaces. and add your gmail id 

```bash
GMAIL_PASSWORD = kfzdrfwibywhriuri
GMAIL = example@gmail.com

```
```bash
const nodemailer = require('nodemailer');
