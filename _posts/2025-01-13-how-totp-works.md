---
layout: post
title: How TOTP Works?
subtitle: A Guide to Time Based One Time Password
gh-repo: Abhishake63/nodejs-totp-authenticator
gh-badge: [star, fork, follow]
tags: [nodejs, totp]
author: Abhishake Sen Gupta
---

## **Introduction**

### Understanding Authentication and 2FA

- **Authentication**: Users prove identity with credentials like email/password or OTPs via email/phone.
- **Risks**: Methods can be compromised; database leaks expose credentials.
- **2FA**: Combines multiple factors to enhance security, making unauthorized access harder.

### Benefits of 2FA

- **Enhanced Security**: Strengthens account protection.
- **Improved User Experience**: Simplifies password recovery and boosts confidence.
- **Various Methods**: Includes OTPs via email, SMS, or biometrics.

### TOTP: A Popular 2FA Method

- **TOTP**: Generates unique, time-sensitive codes, adding an extra security layer.

![image.png](https://abhishake63.github.io/assets/img/totp/image.png)

![image.png](https://abhishake63.github.io/assets/img/totp/image%201.png)

## What is TOTP?

- **TOTP (Time-Based One-Time Password)**: Generates unique passwords for each login attempt using time as a counter.
- **Interval**: New password every 30 seconds.
- **Advantages**: Addresses issues with traditional passwords (forgotten, stolen, guessed).
- **OTPs via SMS/Email**: Can be unreliable and introduce new attack vectors.
- **TOTP Codes**: Generated offline, secure, and convenient.
- **Authenticator App**: Needed on your phone, no internet required.

## OTP vs HOTP vs TOTP: How Each of These Differ?

### OTP (One-Time Password)

- **Definition**: Valid for one session or transaction.
- **Use Cases**: Sent via email or SMS for single-use verification.

![image.png](https://abhishake63.github.io/assets/img/totp/image%202.png)

### HOTP (HMAC-Based One-Time Password)

- **Definition**: Uses a counter and a shared secret key.
- **Mechanism**: Counter increments after each use.
- **Use Cases**: Ideal for hardware tokens where clocks are not synchronized.

![image.png](https://abhishake63.github.io/assets/img/totp/image%203.png)

### TOTP (Time-Based One-Time Password)

- **Definition**: Builds on HOTP by incorporating the current time.
- **Mechanism**: Generates passwords based on fixed time intervals (e.g., 30 seconds).
- **Use Cases**: Commonly used in 2FA apps like Google Authenticator.

![image.png](https://abhishake63.github.io/assets/img/totp/image%204.png)

## **SMS OTP vs. TOTP: Where does TOTP Shine?**

### SMS OTP

- **Advantages**: Easy to implement and use.
- **Disadvantages**: Vulnerable to SIM swapping and interception, potential delays.

### **TOTP**

- **Advantages**: More secure, doesn’t rely on external communication channels.
- **Disadvantages**: Requires an app or hardware token, and there are potential synchronization issues.

![image.png](https://abhishake63.github.io/assets/img/totp/image%205.png)

## How Does TOTP Work?

### Technical Breakdown

- **Shared Secret**: A unique, random string generated when TOTP is enabled. Stored securely on both server and client, often encoded in a QR code. Typically at least 128 bits long.
- **Current Time**: Server and client use the current time, divided into intervals (e.g., 30 seconds), to sync codes.
- **HMAC Algorithm**: Combines the shared secret and current time to produce a hash using HMAC (e.g., SHA-1). Ensures message integrity and authenticity.
- **Password Generation**: Extracts a portion of the hash to create a numerical TOTP code, valid for the current interval. Typically six to eight digits long.

### **Advantages of TOTP**:

- **Offline Generation**: Enhances security and convenience.
- **No Internet Required**: Only need an authenticator app on your phone.
- **Cost-Effective**: Eliminates the need for SMS or email delivery infrastructure.

### **Best Practices for TOTP Integration**

- **Secure Storage**: Use HSMs or encrypted databases for shared secrets.
- **User Education**: Provide clear setup and usage guidance.
- **Clock Synchronization**: Sync clocks with reliable sources.
- **Limit Attempts**: Implement rate limiting and lockout mechanisms.

## Benefits & Limitations of TOTP

### Benefits

- **Security**: Adds an extra layer of security, making it harder for hackers to access accounts. Unique codes are not sent over a network, reducing interception risk.
- **Convenience**: Codes are generated locally on your mobile device, requiring no internet or network access.
- **Cost**: No infrastructure costs for delivery, as TOTP uses an open-source algorithm.

### Limitations

- **Secret Key Storage**: The secret key is stored on both the user’s device and the server. If either is compromised, a malicious actor could generate codes and access the account.

## Emerging Technologies in Two-Factor Authentication

### Additional Authentication Methods

- **Biometric Authentication**: Uses physical attributes like fingerprints, facial recognition, or voice recognition. Examples include Apple’s Face ID and Touch ID, which offer enhanced security through unique personal characteristics.
- **Push Notifications**: Sends authentication requests to trusted devices for approval with a single tap. Example: Google’s 2-Step Verification sends a push notification to your registered device for login approval.
- **Hardware Tokens**: Physical devices like YubiKeys that generate or store authentication codes. These require physical possession, reducing the risk of remote attacks.

### Takeaway

- **TOTP**: Enhances authentication systems by generating unique, time-sensitive codes, addressing issues with passwords and OTPs for a more secure authentication experience.

## Extra

### Y2K Problem

- **Definition**: A computer flaw where systems represented four-digit years with only the last two digits, making the year 2000 indistinguishable from 1900.
- **Cause**: To save memory and storage, years were abbreviated to two digits (e.g., “99” for 1999).
- **Potential Impact**: Feared that systems would fail to recognize the year 2000, leading to errors in date-sensitive operations across various sectors.
- **Preparations**: Extensive global efforts to update and fix systems, with billions spent to ensure Y2K compliance.
- **Outcome**: Few major problems occurred due to the extensive preemptive work by programmers and IT professionals.
- **Lessons Learned**: Highlighted the importance of forward-thinking in software design and the need for regular updates and maintenance of computer systems.

### Year 2038 Problem

- **Definition**: A time-related bug in computing systems using a 32-bit signed integer to represent time since January 1, 1970 (Unix epoch).
- **Critical Date**: January 19, 2038, at 03:14:07 UTC.
- **Issue**: The 32-bit integer will overflow, rolling over to a negative number.
- **Potential Impact**: Systems may interpret the time as December 13, 1901, causing crashes, incorrect calculations, or system failures.

### Solution

- **Upgrade to 64-bit Time Representation**: Using a 64-bit time representation (like `time_t` in modern systems) extends the overflow point billions of years into the future, resolving the issue.