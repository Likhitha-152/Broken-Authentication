# Broken-Authentication
**Broken Authentication** is a vulnerability that occurs when a web application or service fails to properly implement security measures to ensure that only authenticated and authorized users can access resources. When authentication mechanisms are weak, attackers can gain unauthorized access to sensitive information, take over accounts, or impersonate other users. Broken authentication can lead to severe consequences such as data breaches and privilege escalation.

### **Types of Broken Authentication Vulnerabilities**

1. **Weak Password Policies**:
   Weak password policies allow attackers to guess or crack passwords easily.
   
2. **Credential Stuffing**:
   Using stolen usernames and passwords from other breaches to attack a target application.

3. **Session Fixation**:
   When an attacker sets a session ID for a user before they log in, allowing the attacker to take over the session after login.

4. **Session Hijacking**:
   When an attacker steals an active session cookie from a user to impersonate them.

5. **Excessive Session Timeouts**:
   If sessions are not properly expired after a period of inactivity, it can lead to unauthorized access.

6. **Unprotected Password Reset Functionality**:
   If the password reset mechanism isn't secure, attackers can easily reset other users' passwords.

---

### **Broken Authentication Examples**

#### 1. **Weak Password Policy**
A weak password policy may allow users to set passwords that are easily guessable, such as `123456`, `password`, or even `admin`. Attackers can use brute-force or dictionary attacks to guess these weak passwords.

##### **Example:**
A web application doesn't enforce minimum password strength. The user can choose a password like `12345` or `password`.

**Solution**:
Ensure a strong password policy is enforced that includes requirements for:
- Minimum length (e.g., 8 characters)
- Mixed case letters
- Numbers
- Special characters

---

#### 2. **Credential Stuffing Attack**
Credential stuffing attacks occur when attackers use automated scripts to try previously breached username and password combinations against a target website.

##### **Example:**
An attacker might have obtained a list of usernames and passwords from a previous data breach (e.g., from `Have I Been Pwned`) and attempt to log into a target website using this list.

```bash
# Example of an automated credential stuffing attack using Python and the requests library
import requests

# Target website
url = "https://target-website.com/login"

# List of usernames and passwords from a data breach
credentials = [
    ("user1", "password123"),
    ("user2", "admin123"),
    ("user3", "123456"),
]

# Attempt login for each credential pair
for username, password in credentials:
    response = requests.post(url, data={"username": username, "password": password})
    if "Invalid credentials" not in response.text:
        print(f"Successful login with {username}:{password}")
    else:
        print(f"Failed login for {username}:{password}")
```

**Solution**:
Implement **rate-limiting** and **CAPTCHAs** to prevent automated login attempts, and encourage the use of strong, unique passwords through password strength checks and multi-factor authentication (MFA).

---

#### 3. **Session Fixation**
An attacker can set a session ID for a user before they log in. If the user then logs in, the attacker can hijack the session by using the same session ID.

##### **Example:**
1. The attacker sets the session ID for the user, such as `SESSIONID=12345`.
2. The attacker sends the victim a link like: `http://target-website.com/login?SESSIONID=12345`.
3. Once the victim logs in, the attacker uses the same session ID (`12345`) to hijack the victim's session.

**Solution**:
- Use **secure session management** that regenerates session IDs after login (i.e., on authentication, the server issues a new session ID).
- Set the `HttpOnly` and `Secure` flags on session cookies to ensure they are sent only over HTTPS and are not accessible via JavaScript.

```javascript
document.cookie = "SESSIONID=12345; Secure; HttpOnly";
```

---

#### 4. **Session Hijacking**
Session hijacking occurs when an attacker steals a user's session cookie (e.g., via Cross-Site Scripting or network interception) and impersonates them.

##### **Example:**
An attacker uses a **Man-in-the-Middle (MITM)** attack or **Cross-Site Scripting (XSS)** to steal the session cookie of a user who is logged in to a web application. The attacker then uses the stolen session to gain unauthorized access.

**Solution**:
- **Use HTTPS** to encrypt the communication between the client and server.
- Use **Secure** and **HttpOnly** flags in session cookies.
- Implement **SameSite cookies** to prevent cross-site request forgery (CSRF) attacks.

```javascript
document.cookie = "SESSIONID=12345; Secure; HttpOnly; SameSite=Strict";
```

---

#### 5. **Excessive Session Timeout**
If the application does not properly expire a session after a period of inactivity, an attacker who gains access to a victim's computer could use an open session to perform unauthorized actions.

##### **Example:**
- User logs in and leaves the computer idle for hours.
- The session is still active and the attacker can perform actions as the logged-in user.

**Solution**:
- Implement **session expiration** after a certain period of inactivity.
- Provide an option for users to log out automatically after a predefined session timeout.

```javascript
// Example of setting a session timeout of 15 minutes
setTimeout(function() {
    alert('Session expired due to inactivity');
    window.location.href = '/logout';
}, 900000); // 900000ms = 15 minutes
```

---

#### 6. **Unprotected Password Reset Functionality**
If the password reset functionality is not properly secured, an attacker could easily reset another user's password and gain unauthorized access.

##### **Example:**
The password reset mechanism could be easily abused if the application does not require a secondary step, such as an email confirmation or multi-factor authentication (MFA), before resetting a password.

**Solution**:
- **Verify the user's identity** before allowing password resets.
- Send a one-time password (OTP) to the user's email or phone number before allowing the reset.
- Implement CAPTCHA or rate-limiting to prevent brute-force attacks on the reset mechanism.

```javascript
// Example of email-based password reset
app.post('/reset-password', (req, res) => {
    const { email } = req.body;
    // Verify email and send password reset link with a unique token
    sendResetLink(email);
});
```

---

### **How to Prevent Broken Authentication**

1. **Enforce Strong Password Policies**: Require users to create strong passwords with a combination of letters, numbers, and special characters.
   
2. **Implement Multi-Factor Authentication (MFA)**: Even if credentials are compromised, MFA adds an extra layer of protection.

3. **Use Secure Session Management**: Use secure cookies (`HttpOnly`, `Secure`, `SameSite`), regenerate session IDs on login, and set session expiration times.

4. **Rate Limit and CAPTCHA**: Implement rate-limiting for login attempts and use CAPTCHA systems to prevent brute-force and credential stuffing attacks.

5. **Protect Password Reset Functions**: Ensure password reset mechanisms are protected by additional steps (e.g., email confirmation, CAPTCHA, or MFA).

6. **Monitor and Log Authentication Events**: Monitor login attempts and access to sensitive resources to detect suspicious activities.

---

### **Conclusion**
Broken authentication is a critical security flaw that can be exploited by attackers to gain unauthorized access to sensitive data, hijack accounts, or escalate privileges. To mitigate the risk of broken authentication vulnerabilities, implement secure authentication mechanisms, such as strong password policies, multi-factor authentication, and secure session management practices.
