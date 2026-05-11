# TheCyberForum - DevSecOps Security Analysis Project

**Version:** 1.1.0 (Security Enhanced Edition)

A Flask-based blog application demonstrating comprehensive DevSecOps practices, including threat modeling, SAST, DAST, and automated security scanning in CI/CD pipelines.

## Project Overview

This project showcases the integration of security analysis into the software development lifecycle (SDLC) through:
- STRIDE threat modeling using OWASP Threat Dragon
- Automated SAST (Static Application Security Testing) with Bandit
- Automated DAST (Dynamic Application Security Testing) with OWASP ZAP
- CI/CD security automation via GitHub Actions
- Security header implementation and vulnerability mitigation

## Security Features Implemented

### Application-Level Security
- **CSRF Protection**: Flask-WTF token validation on all POST requests
- **Security Headers**: 
  - X-Frame-Options (clickjacking prevention)
  - X-XSS-Protection
  - Content-Security-Policy (CSP)
- **Session Security**: File-based session management with configurable lifetime
- **Input Validation**: HTML tag stripping via custom template filters
- **SQL Injection Prevention**: Parameterized queries throughout

### Automated Security Testing

| Test Type | Tool | Trigger | Output |
|-----------|------|---------|--------|
| SAST | Bandit | Push/Pull Request | HTML/Text Reports |
| DAST | OWASP ZAP | Push/Pull Request | HTML Report on GitHub Pages |
| Threat Model Validation | Owasp Threat dragon | Threat model changes | JSON Validation |

## Threat Model (STRIDE Analysis)

The application's threat model is documented using OWASP Threat Dragon, covering:

### Trust Boundaries
- **Internet Boundary**: Between users and web application
- **Data Boundary**: Between web application and database

### Components Analyzed
1. **User Interface/Browser** - Jinja2 templates
   - Threats: Spoofing (phishing), Repudiation (CSRF mitigated)
   
2. **Login Session** - Session/cookie management
   - Threats: Session hijacking (Open), Logging deficit (Mitigated)
   
3. **Flask Web App** - Core application logic
   - Threats: MitM (Open), Clickjacking (Mitigated), Info disclosure (Open)
   
4. **API Endpoints** - RESTful/ad-hoc endpoints
   - Threats: Data exposure (Open), DoS (Open), IDOR (Open)
   
5. **Database** - SQLite storage
   - Threats: SQL injection (Open), Plaintext storage (Open)

### Threat Status Summary
- **Open Threats**: Session hijacking, SQL injection, Information disclosure, DoS, IDOR
- **Mitigated Threats**: CSRF (Flask-WTF), Clickjacking (X-Frame-Options)
- **Risk Scores**: Ranging from 2.6 (Low) to 6.8 (Medium)

## CI/CD Security Pipeline

### GitHub Actions Workflows

1. **SAST Scan (`sast-scan.yml`)**
   - Runs Bandit security scanner on all Python code
   - Generates HTML and text reports
   - Uploads reports as artifacts (30-day retention)

2. **DAST Scan (`dast-scan.yml`)**
   - Spins up Flask application with Gunicorn
   - Executes OWASP ZAP baseline scan
   - Deploys HTML report to GitHub Pages
   - Falls back to artifact upload on failure

3. **Threat Model Validation (`validate-threat-model.yml`)**
   - Validates JSON structure of threat model
   - Triggers on changes to Threat Dragon files

4. **Code Quality (`pylint.yml`)**
   - Runs Pylint across multiple Python versions (3.8-3.10)

5. **CI Build (`deploy.yml`)**
   - Installs dependencies
   - Starts application
   - Executes test suite

## Threat Mitigation Status

### Mitigated Vulnerabilities
✅ **CSRF Protection** - Flask-WTF token validation implemented  
✅ **Clickjacking** - X-Frame-Options: SAMEORIGIN configured  
✅ **XSS Protection** - X-XSS-Protection header enabled  
✅ **Content Security Policy** - Restrictive CSP policy configured  
✅ **Repudiation** - Request logging via CSRF tokens

### Open Vulnerabilities (Requiring Remediation)
⚠️ **Session Hijacking** - Need HTTPS enforcement, secure cookie flags  
⚠️ **SQL Injection** - Some raw queries need ORM migration  
⚠️ **Plaintext Passwords** - Implement bcrypt/Argon2 hashing  
⚠️ **Information Disclosure** - Implement DTOs for API responses  
⚠️ **Rate Limiting** - Add rate limiting for login/search endpoints  
⚠️ **Missing HTTPS** - Enforce HTTPS in production


## Project Structure
```bash
flask_blog/
├── app.py                      # Main Flask application with security headers
├── init_db.py                  # Database initialization
├── schema.sql                  # Database schema
├── requirements.txt            # Python dependencies (includes security tools)
├── .github/workflows/          # CI/CD security pipelines
│   ├── sast-scan.yml          # Bandit SAST automation
│   ├── dast-scan.yml          # OWASP ZAP DAST automation
│   ├── validate-threat-model.yml
│   ├── pylint.yml
│   └── deploy.yml
├── ThreatDragonModels/         # STRIDE threat model
│   └── Website Threat Model/
│       └── Website Threat Model.json
├── static/                     # Static assets
└── templates/                  # Jinja2 templates
```

## Prerequisites

- Python 3.8 or higher
- pip package manager
- Docker (for local ZAP scanning)

## Installation

```bash
# Clone the repository
git clone https://github.com/AugustusNnamdi/flask_blog.git
cd flask-blog

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt

# Initialize database
python init_db.py
```

### Security Testing Checklist
- Threat modeling completed (STRIDE)
- SAST integrated into CI/CD
- DAST integrated into CI/CD
- Security headers implemented
- CSRF protection enabled
- HTTPS/TLS configured
- Password hashing implemented
- Rate limiting added
- SQL injection audit completed
- Session management reviewed


### Resources
- [OWASP Top Ten](https://owasp.org/Top10/)
- [STRIDE Threat Modeling](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- [OWASP Threat Dragon](https://owasp.org/www-project-threat-dragon/)
- [Bandit Security Scanner](https://bandit.readthedocs.io/)
- [OWASP ZAP](https://www.zaproxy.org/)

### License

MIT License - See LICENSE file for details
