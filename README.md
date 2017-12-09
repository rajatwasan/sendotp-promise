## SendOtp-Promise - Node.js SDK

[![Build Status](https://travis-ci.org/ashokdey/sendotp-promise.svg?branch=master)](https://travis-ci.org/ashokdey/sendotp-promise) &nbsp; [![Maintainability](https://api.codeclimate.com/v1/badges/a68b653ec5036055e058/maintainability)](https://codeclimate.com/github/ashokdey/sendotp-promise/maintainability)

This SDK is the promise wrapper for **[SentOtp](https://github.com/MSG91/sendotp-node)** by MSG91

### Set-up:

1. Download the NPM module
```javascript
npm install sendotp -S / yarn add sendotp-promise
```
2. Require the package in your code.
```javascript
const SendOTP = require('sendotp-promise'); / import SendOTP from 'sendotp-promise';
```
3. Initialize with your [MSG91](https://msg91.com) auth key
```javascript
const sendOtp = new SendOTP('AuthKey');
```
That's all, your SDK is set up!

### Requests

You now have the send, retry and verify otp via following methods.
```javascript
sendOtp.send(contactNumber, senderId, otp, callback); //otp is optional if not sent it'll be generated automatically
sendOtp.retry(contactNumber, retryVoice, callback);
sendOtp.verify(contactNumber, otpToVerify, callback);
```

### Usage:

To send OTP, without optional parameters

```javascript
// normal callback
sendOtp.send("919999999999", "PRIIND", function (error, data, response) {
  console.log(data);
});

```

### Using async-await

```javascript

// ES6 import 
import SendOTP from 'sendotp-promise';

// using commonJS pattern
// const SendOTP = require('sendotp-promise');

const MSG91_AUTH_KEY = 'your auth key here';
const MSG91_SENDER_ID = 'your sender id of 6 characters';

// new instance of SendOTP
const sendOtp = new SendOtp(MSG91_AUTH_KEY);

// set the expiry for your OTP
sendOtp.setOtpExpiry('60');

const sendOtpToMobile = async (mobileNumber) => {
  try {
    // call the send() method
    const response = await sendOtp.send(mobile, MSG91_SENDER_ID);
    console.log(response);
    if (response.type === 'success') {
      return console.log('OTP code sent');
    }

    return console.log('Failed to sent OTP');
  } catch (err) {
    console.error(err);
    return console.log('Something went wrong');
  }
};

// the mobile number
const countryCode = 'your country code';
const mobileNumber = 'any mobile number';
const completeMobileNumber = `${countryCode}${mobileNumber}`;

// call 
sendOtpToMobile(completeMobileNumber);
```

To send OTP, with optional parameters
```javascript
sendOtp.send("919999999999", "PRIIND", "4635", function (error, data, response) {
  console.log(data);
});
```

If you want to set custom expiry of OTP verification  
```javascript
sendOtp.setOtpExpiry('90'); //in minutes
```

To retry OTP
```javascript
sendOtp.retry("919999999999", false, function (error, data, response) {
  console.log(data);
});
```
**Note:** In sendOtp.retry() set retryVoice false if you want to retry otp via text, default value is true

To verify OTP
```javascript
sendOtp.verify("919999999999", "4365", function (error, data, response) {
  console.log(data); // data object with keys 'message' and 'type'
  if(data.type == 'success') console.log('OTP verified successfully')
  if(data.type == 'error') console.log('OTP verification failed')
});
```

### Options:

By default sendotp uses default message template, but custom message template can also set in constructor like
```javascript
const SendOtp = require('sendotp');
const sendOtp = new SendOtp('AuthKey', 'Otp for your order is {{otp}}, please do not share it with anybody');
```

`{{otp}}` expression is used to inject generated otp in message.

### Licence:

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
