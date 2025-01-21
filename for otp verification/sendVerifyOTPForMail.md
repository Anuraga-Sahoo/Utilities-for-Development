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

### Step 6
import createTransport nodemailer in sendMail.js file which is a middlewaire and write the logic
```bash
import {createTransport} from 'nodemailer'

const sendMail = async(email, subject, data) => {
  const transport = createTransport({
    host: "smtp.gmail.com",
    port:465,
    auth: {
      user: process.env.GMAIL,
      pass: process.env.GMAIL_PASSWORD,
    }
  })

  const html = `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OTP Verification</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            text-align: center;
        }
        h1 {
            color: red;
        }
        p {
            margin-bottom: 20px;
            color: #666;
        }
        .otp {
            font-size: 36px;
            color: #7b68ee; /* Purple text */
            margin-bottom: 30px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>OTP Verification</h1>
        <p>Hello ${data.name} your (One-Time Password) for your account verification is.</p>
        <p class="otp">${data.otp}</p> 
    </div>
</body>
</html>
`;

  await transport.sendMail({
    from: process.env.GMAIL,
    to: email,
    subject,
    html
  })
}

export default sendMail

```
### step 7
create the user.controllers file.
here i create the registration logic for register a user. in this i use sendMail middlewaire to generate and send otp to users gmail

```bash

import sendMail from "../middlewares/sendMail.js"

export const register = async (req, res) => {
  try {
    const {email, name, password} = req.body
    let user = await User.findOne({email})

    if(user) return res.status(400).json({
      message: "User Already exist",
      success: false,
    })

    const hashPassword = await bcrypt.hash(password, 10)

    user = {
      name,
      email,
      password: hashPassword
    }

    const otp = Math.floor(Math.random()*1000000)
    const activationToken = jwt.sign({user, otp}, process.env.JWT_SECRET,{expiresIn: "5m",} )

    const data = {
      name,
      otp,
    }

    await sendMail(
      email, `Technivers`, data
    )

    res.status(200).json({
      message: "OTP send to your mail",
      activationToken,
      success: true
    })
    
  } catch (error) {
    res.status(500).json({
      message: error.message,
      error: error
    })
  }
}

```

### step 8
Now create verification route or verification user controllers in user.controllers.js file

