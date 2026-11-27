# Exploring Vulnerable password Reset Logic

# Password Reset Mechanisms

## 1\. Introduction to Password Reset Mechanisms

**Definition:** Password reset mechanisms allow users to regain access to their accounts when they forget their passwords.

They are an important part of user convenience in modern web applications. However, poorly secured implementations can be exploited by attackers.

**Primary objective:** Verify the identity of the user before allowing them to change their password.

There are three common password reset methods:

1.  Email-Based Reset
    
2.  Security Question-Based Reset
    
3.  SMS-Based Reset
    

* * *

## 2\. Types of Password Reset Mechanisms

### 2.1 Email-Based Reset

**Definition:** The application sends a password reset link or token to the user's registered email address.

**How it works:**

1.  The user requests a password reset.
    
2.  The application sends an email containing a reset link or token.
    
3.  The user clicks the link.
    
4.  The link directs the user to a password reset page.
    
5.  The user enters and confirms a new password.
    

Alternatively, the system may automatically generate a new password.

**Security consideration:**

This method relies heavily on:

- The security of the user's email account.
    
- The secrecy of the reset link or token.
    

### 2.2 Security Question-Based Reset

**Definition:** The user answers pre-configured security questions established when creating their account.

**How it works:**

1.  The user requests a password reset.
    
2.  The application presents previously configured security questions.
    
3.  The user provides answers.
    
4.  The application verifies the answers.
    
5.  If correct, the user is allowed to reset their password.
    

**Security consideration:**

This method assumes that only the legitimate user knows the answers.

However, it can be compromised if an attacker obtains personally identifiable information (PII), which may be publicly available, easily discovered or guessed.

### 2.3 SMS-Based Reset

**Definition:** The application sends a password reset code or link directly to the user's registered mobile phone through SMS.

**How it works:**

1.  The user requests a password reset.
    
2.  The application sends a reset code or link through SMS.
    
3.  The user receives the code.
    
4.  The user enters the code on the provided webpage.
    
5.  The application verifies the code and grants access to the password reset functionality.
    

**Security consideration:**

This method assumes that access to the user's phone is secure.

However, it can be vulnerable to:

- SIM swapping attacks.
    
- SMS interception.
    

* * *

## 3\. Common Password Reset Vulnerabilities

Each password reset mechanism has potential security weaknesses.

### 3.1 Predictable Tokens

**Definition:** Predictable tokens are reset tokens that follow patterns or sequences that attackers may be able to guess.

For example, if reset tokens are sequential:

```
Token 1: 10001
Token 2: 10002
Token 3: 10003
```

An attacker might predict another valid token.

**Security weakness:** Attackers may guess or brute-force valid reset URLs or tokens.

### 3.2 Token Expiration Issues

**Definition:** Token expiration determines how long a password reset token remains valid.

Problems occur when:

- Tokens remain valid for too long.
    
- Tokens do not expire after being used.
    

This gives attackers a larger window of opportunity to exploit stolen or exposed tokens.

**Security consideration:** Tokens should expire quickly to limit the opportunity for misuse.

### 3.3 Insufficient Validation

**Definition:** Insufficient validation occurs when the application does not properly verify a user's identity before allowing a password reset.

Examples include:

- Weak security questions.
    
- Easily guessed answers.
    
- Compromised email accounts.
    

**Security weakness:** An attacker may bypass the intended identity verification process.

### 3.4 Information Disclosure

**Definition:** Information disclosure occurs when an application unintentionally reveals sensitive information through its responses.

Example:

```
Email: user@example.com

Response: Email address not registered.
```

This tells an attacker that the supplied email address does not belong to an existing account.

**Security weakness:** Attackers can use these responses for account enumeration.

### 3.5 Insecure Transport

**Definition:** Insecure transport occurs when sensitive information is transmitted without adequate encryption.

For example, transmitting reset links or tokens over HTTP instead of HTTPS.

**Security weakness:** Network eavesdroppers may intercept sensitive reset information.

# Exploiting Predictable Tokens

Link to predictions lab:  [http://enum./labs/predictable\_tokens/](http://enum.thm/labs/predictable_tokens/).

![72711ce742069510545825c556edeba2.png](../_resources/72711ce742069510545825c556edeba2.png)

&nbsp;

Tokens that are simple, predictable, or have long expiration times can be particularly vulnerable to interception or brute force.

For example, the below code is used by the vulnerable application hosted in the Predictable Tokens lab:

```
$token = mt_rand(100, 200);
$query = $conn->prepare("UPDATE users SET reset_token = ? WHERE email = ?");
$query->bind_param("ss", $token, $email);
$query->execute();
```

The code above sets a random three-digit PIN as the reset token of the submitted email. Since this token doesn't employ mixed characters, it can be easily brute-forced.![9d14fc60801a4cde91cf3f6bb72c21eb.png](../_resources/9d14fc60801a4cde91cf3f6bb72c21eb.png)

The web application uses the reset link: `http://enum.thm/labs/predictable_tokens/reset_password.php?token=123`

![66b324886b1128fcd6df6fe4f36fbfc4.png](../_resources/66b324886b1128fcd6df6fe4f36fbfc4.png)

the token is a simple numeric value. Using Burp Suite, navigate to the above URL and capture the request. Subsequently, send the request to the Intruder, highlight the value of the token parameter, and click the Add payload button.

# Task 2 Using Burpsuite and crunch to bruteforce

user@tryhackme $ crunch 3 3 -o otp.txt -t %%% -s 100 -e 200

Crunch will now generate the following amount of data: 404 bytes  
0 MB  
0 GB  
0 TB  
0 PB  
Crunch will now generate the following number of lines: 101

crunch: 100% completed generating output

&nbsp;![dd4bc17f176a1b8f5bcf1a4df10e792d.png](../_resources/dd4bc17f176a1b8f5bcf1a4df10e792d.png)
```
&nbsp;

&nbsp;

&lt;html lang="en"&gt;  
&lt;head&gt;  
&lt;meta charset="UTF-8"&gt;  
&lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;  
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;  
&lt;link rel="stylesheet" href="styles.css"&gt;  
&lt;script src="jquery.min.js"&gt;&lt;/script&gt;  
&lt;title&gt;Reset Password&lt;/title&gt;  
&lt;/head&gt;  
&lt;body&gt;  
&lt;div class="container"&gt;  
&lt;div class="content"&gt;  
&lt;h1&gt;Reset Password&lt;/h1&gt;  
&lt;div class="column-50"&gt;  
&lt;p id="messages"&gt;  
&lt;p class="succ"&gt;Your new password is: B7nDc5q6&lt;/p&gt;&lt;p class="succ"&gt;Email: admin@admin.com&lt;/p&gt;
```

![1e2cb286297d3092ec6c8f3f98233ca7.png](../_resources/1e2cb286297d3092ec6c8f3f98233ca7.png)