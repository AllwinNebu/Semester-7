```markdown
# Authentication & Authorization Testing Lab

## Experiment No: 1
### Title: Security Testing of Authentication Mechanisms in Web Applications

### Objective
To identify and exploit common authentication vulnerabilities including:
- Default credentials
- Weak lockout mechanisms  
- Browser cache issues
- Weak password policies
- Insecure password reset functionalities

---

## Lab Setup & Environment

### Lab Application Details
- **URL**: http://localhost:5000
- **Technology Stack**: Flask, PostgreSQL, Docker
- **Purpose**: Deliberately vulnerable web application for security education

---

## Experiment Procedures

### TEST 1: Default Credentials Testing
**Aim**: To identify accessible accounts using common default credentials.

**Procedure**:
```bash
# Create credential files
echo -e "admin\nuser\ntest\ndemo\nguest\nroot" > users.txt
echo -e "admin\npassword\ntest\ndemo\n123456\nletmein" > passwords.txt

# Execute Hydra attack
hydra -L users.txt -P passwords.txt -f -V -s 5000 localhost http-post-form "/login:username=^USER^&password=^PASS^:F=Invalid"
```

**Results**:
| Username | Password | Status |
|----------|----------|---------|
| admin    | admin    | ✅ Success |
| user     | password | ✅ Success |
| test     | test     | ✅ Success |
| demo     | demo     | ✅ Success |

**Finding**: Multiple default credentials provide unauthorized access.

---

### TEST 2: Weak Lockout Mechanism Testing
**Aim**: To test account lockout policies and brute force protection.

**Procedure**:
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt -t 10 -W 1 -f -s 5000 localhost http-post-form "/login:username=^USER^&password=^PASS^:F=locked"
```

**Observations**:
- Attempts: 1000+ rapid login attempts
- Lockout: No account lockout detected
- Rate Limiting: No throttling observed
- Duration: Continuous attack possible

**Finding**: Weak lockout mechanism allows unlimited brute force attempts.

---

### TEST 3: Browser Cache Weakness Testing
**Aim**: To identify improper cache controls on sensitive pages.

**Procedure**:
```bash
curl -s -I http://localhost:5000/profile | grep -i "cache-control\|pragma\|expires" || echo "NO CACHE HEADERS FOUND"
```

**Observations**:
- Cache-Control Header: ❌ Absent
- Pragma Header: ❌ Absent
- Expires Header: ❌ Absent
- Security Headers: ❌ Not implemented

**Manual Verification Steps**:
1. Login → Access /profile → Logout
2. Press browser back button → Cached content accessible
3. Check Developer Tools → No cache directives

**Finding**: Sensitive pages lack cache control, exposing data after logout.

---

### TEST 4: Weak Password Policy Testing
**Aim**: To evaluate password complexity requirements.

**Procedure**:
```bash
# Test weak password acceptance
curl -X POST http://localhost:5000/register -d "username=weakuser1&email=weak1@test.com&password=1&confirm_password=1" -s | grep -q "successful" && echo "WEAK PASSWORD ACCEPTED" || echo "WEAK PASSWORD REJECTED"
```

**Password Policy Analysis**:
| Password Type | Accepted | Example |
|---------------|----------|---------|
| 1 character   | ✅ Yes    | "1"     |
| 2 characters  | ✅ Yes    | "ab"    |
| 3 characters  | ✅ Yes    | "abc"   |
| Common passwords | ✅ Yes | "password" |

**Policy Deficiencies**:
- Minimum Length: 3 characters ❌
- Complexity: No requirements ❌
- Dictionary Check: Not implemented ❌

**Finding**: Extremely weak password policy allows easily guessable passwords.

---

### TEST 5: Weak Password Reset Functionality
**Aim**: To identify vulnerabilities in password recovery mechanism.

**Procedure**:
```bash
# Test user enumeration
curl -X POST http://localhost:5000/reset_password -d "email=nonexistent@test.com" -s | grep -q "No account found" && echo "USER ENUMERATION POSSIBLE" || echo "NO ENUMERATION"

# Test token security
wfuzz -c -z range,00000000-99999999 --hc 404 "http://localhost:5000/reset_password_confirm/FUZZ"
```

**Observations**:

**A. Information Disclosure**:
- Response differentiation for valid/invalid emails
- Error: "No account found with that email address"

**B. Reset Token Analysis**:
- Token exposed in response ❌
- No token expiration ❌
- Predictable token pattern ❌

**C. Security Flaws**:
- No rate limiting on reset requests
- Tokens displayed in clear text
- No account lockout for reset attempts

**Finding**: Password reset mechanism vulnerable to enumeration and token attacks.

---

## Vulnerability Summary

| Vulnerability | Severity | Impact | Exploitation Complexity |
|---------------|----------|---------|-------------------------|
| Default Credentials | High | Full system compromise | Low |
| Weak Lockout | High | Account takeover | Low |
| Browser Cache Issues | Medium | Information disclosure | Low |
| Weak Password Policy | Medium | Credential cracking | Low |
| Insecure Password Reset | High | Account takeover | Medium |

---

## Security Recommendations

### Immediate Actions:
1. **Change all default credentials**
2. **Implement account lockout** after 5 failed attempts
3. **Add proper cache control headers** for sensitive pages
4. **Enforce strong password policy** (minimum 8 characters, complexity requirements)
5. **Secure password reset** with time-limited, unpredictable tokens

### Technical Controls:
- Implement HTTPS for all communications
- Add security headers (HSTS, CSP)
- Use secure session management with proper expiration
- Conduct regular security audits and penetration testing

---

## Conclusion

The lab successfully demonstrated five critical authentication vulnerabilities commonly found in web applications. Each vulnerability was exploitable using standard Kali Linux tools, highlighting the importance of robust authentication mechanisms. The experiment provided hands-on experience in both identifying and understanding the impact of these security flaws.

### Key Takeaways:
- Default credentials pose immediate security risks
- Account lockout mechanisms are essential for brute force protection
- Proper cache controls prevent sensitive data exposure
- Strong password policies are fundamental to security
- Password reset functionality requires careful implementation

This lab emphasizes the critical need for comprehensive authentication security measures in production web applications.
```
