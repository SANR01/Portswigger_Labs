# SQL Injection Vulnerability Allowing Login Bypass

## Objective
The objective of this lab was to exploit a SQL Injection vulnerability in the login functionality and bypass authentication without knowing the correct password.

---

## Lab Overview
The application contains a login form that verifies user credentials using a backend SQL query.

A normal login process checks both the username and password before granting access.

Example of a normal query:

sql id="normalquery"
SELECT * FROM users <br>
WHERE username='administrator' <br>
AND password='mypassword'

## Recon

I used Burp Suite to intercept the login request and observe how the application handled user input.

The request parameters appeared to be directly processed inside a SQL query.

## Problem Faced

Initially, I was unsure how the backend authentication query was structured.

The challenge was finding a payload that could manipulate the SQL query logic and bypass the password verification process.

## Exploitation

Using Burp Suite, I intercepted the login request and modified the username parameter with the following payload:

administrator'--

## Payload Explanation

administrator → target account username <br>
' → closes the SQL string <br>
-- → comments out the remaining part of the SQL query

This caused the password verification section of the query to be ignored.

## Modified query behavior:

SELECT * FROM users <br>
WHERE username='administrator'-- <br> 
AND password='mypassword'

The application ignored everything after --, including the password check.

## Result

The authentication process was successfully bypassed, and access to the administrator account was achieved without knowing the correct password.

## Impact

Unauthorized account access <br>
Authentication bypass vulnerability <br>
Potential administrative account compromise

## Mitigation
Use parameterized queries (prepared statements) <br>
Validate and sanitize user input <br>
Avoid directly concatenating user input into SQL queries <br>
Implement secure authentication handling

## Learning Outcome
Learned how SQL Injection affects login systems <br>
Practiced request interception using Burp Suite <br>
Understood authentication bypass techniques <br>
Improved understanding of insecure SQL query handling
