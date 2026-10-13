# Lab: Forced OAuth Profile Linking

**Difficulty:** Practitioner

**Status:** Solved

---

## Lab Overview

This lab contains a vulnerability in the OAuth profile-linking functionality of a blog application.

The application allows users to link their social media profile to their existing blog account so that they can log in using their social media identity instead of their username and password.

However, the OAuth account-linking flow does not adequately protect against Cross-Site Request Forgery (CSRF). An attacker can exploit this weakness to force an administrator to link the attacker's social media profile to the administrator's blog account.

The objective of the lab is to exploit this vulnerability, access the administrator's account, and delete the user `carlos`.

---

## Tools Used

- Burp Suite Community Edition
- PortSwigger Web Security Academy
- PortSwigger Exploit Server

---

## Vulnerability

1. The application allows users to link an external social media profile to their existing blog account through OAuth.
2. The account-linking process does not adequately validate whether the action was intentionally initiated by the authenticated user.
3. An attacker can exploit the missing or ineffective CSRF protection to trigger the account-linking process in a victim's authenticated browser.

---

## Affected Functionality

The affected functionality is the **Attach your social media** feature, which links a social media identity to an existing blog account.

---

## Root Cause

1. The OAuth account-linking flow lacks effective CSRF protection.
2. The application does not adequately bind the linking operation to the user's intended action and the correct browser session.
3. The application allows an attacker-controlled social media identity to be linked to another user's account through a forged request.

---

## Testing Steps

### 1. Testing the Login Functionality

First, I accessed the blog application and observed two login options:

- Log in using a username and password.
- Log in using a social media profile.

I initially tested the social media login functionality but did not identify the vulnerability at this stage.

I then logged in using the provided blog account credentials:

- Username: `wiener`
- Password: `peter`

After logging in, I navigated to the account functionality and found the option to attach a social media profile.

### 2. Analyzing the Account-Linking Requests

I enabled Burp Suite Proxy and intercepted the requests generated when clicking **Attach your social media**.

The process generated multiple requests, including:

- A request to load the account-linking functionality.
- An OAuth authorization request containing parameters such as `client_id`, `redirect_uri`, `response_type`, and `scope`.
- A subsequent request associated with consent and authorization-code generation.

I examined the requests and responses to understand how the application handled the OAuth linking process.

During testing, I also modified the `Origin` header in one request. This was part of my investigation into the application's request validation, rather than proof of a vulnerability by itself.

### 3. Identifying the CSRF Vulnerability

I investigated whether the profile-linking operation could be triggered without sufficient protection against cross-site requests.

The critical weakness was that the application did not adequately ensure that the account owner had intentionally initiated the linking operation.

Because the administrator was assumed to have an active session on the blog application, a malicious page could potentially trigger the vulnerable flow in the administrator's browser.

### 4. Capturing the OAuth Linking Flow

I completed the profile-linking process using my own social media test account and examined the generated authorization and consent requests.

I captured the relevant URL and authorization values required by the lab's linking flow.

This helped me understand how the application connected the external social media identity to the currently authenticated blog account.

### 5. Exploiting the Vulnerability Using the Exploit Server

I used the PortSwigger Exploit Server to host an HTML page containing an iframe that triggered the relevant captured OAuth linking URL.

I then used the lab's delivery mechanism to send the exploit to the administrator.

When the administrator visited the page with an active blog session, the forged request triggered the vulnerable account-linking flow.

The administrator's account became associated with my social media profile.

**Evidence:** Add your Burp Suite request/response screenshots and the exploit-server screenshot here.

### 6. Accessing the Administrator Account

After successfully linking my social media profile to the administrator's blog account, I used the social media login functionality to access the administrator's account.

I navigated to the administration panel and deleted the user `carlos`.

The lab displayed the success confirmation, demonstrating that the account-linking vulnerability had been exploited successfully.

---

## Impact

A successful forced OAuth profile-linking attack may allow an attacker to:

1. Associate their own external identity with another user's account.
2. Authenticate as the victim through the linked social media profile.
3. Gain unauthorized access to sensitive account information and functionality.
4. Perform privileged actions if the compromised account has administrative permissions.

In this lab, the demonstrated impact was administrator account compromise and unauthorized deletion of the user `carlos`.

---

## Remediation

1. **Implement CSRF protection:** Protect sensitive account-linking operations with unpredictable CSRF tokens and server-side validation.
2. **Validate OAuth state:** Generate a cryptographically random `state` value for each authorization transaction and validate it against the initiating browser session.
3. **Bind the linking operation to the correct session:** Ensure that the external identity is linked only to the account that intentionally initiated the process.
4. **Require explicit confirmation:** Clearly display the social media identity being linked and require the user to confirm the operation.
5. **Validate the OAuth response:** Verify that the authorization response belongs to the correct transaction and account-linking attempt.
6. **Use secure session handling:** Configure session cookies appropriately, but do not rely on cookie attributes as the sole CSRF defense.

---

## OWASP / CWE Reference

- **OWASP:** Cross-Site Request Forgery (CSRF)
- **CWE-352:** Cross-Site Request Forgery (CSRF)
- **OWASP Web Security Testing Guide:** Testing for OAuth weaknesses and account-linking vulnerabilities

---

## What I Learned

1. **OAuth authentication vs. account linking:** OAuth-related flows can introduce vulnerabilities even when the normal login process appears to work correctly.
2. **CSRF in account linking:** Sensitive operations such as linking an external identity require protection against forged cross-site requests.
3. **OAuth authorization flow:** I learned to identify important parameters such as `client_id`, `redirect_uri`, `response_type`, and `scope` when analyzing authorization requests.
4. **Session-dependent attacks:** An attacker may exploit a victim's active authenticated session to trigger an unwanted account change.
5. **Practical exploitation with Burp Suite:** I practiced intercepting and analyzing requests, examining OAuth flows, and using the PortSwigger Exploit Server to test a CSRF attack.
6. **Impact beyond login:** An insecure account-linking implementation can lead to account takeover and privilege abuse, including administrative actions.

---
