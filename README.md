# Auto-Security-Scanner 🛡️🔍

**[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)**
**[![Build Status](https://github.com/Hemaksh69/Auto-Security-Scanner/actions/workflows/test.yml/badge.svg)](https://github.com/Hemaksh69/Auto-Security-Scanner/actions)**
**[![Shell Check](https://github.com/koalaman/shellcheck/actions/workflows/shellcheck.yml/badge.svg)](https://github.com/koalaman/shellcheck)**
**[![Code Style: ShellCheck](https://img.shields.io/badge/code%20style-shellcheck-000000.svg)](https://github.com/koalaman/shellcheck)**

> *"Security should be automatic, not optional. Scan your systems effortlessly with Auto-Security-Scanner."*

---

## 🚀 **Overview**

Auto-Security-Scanner is a **lightweight, open-source Bash script** designed to help developers, sysadmins, and security enthusiasts quickly assess potential vulnerabilities in their systems. Whether you're checking a local machine, a web server, or a Docker container, this tool provides a **clear, actionable threat assessment report** with minimal effort.

### **Why Auto-Security-Scanner?**
✅ **Lightweight & Fast** – No heavy dependencies, runs in seconds.
✅ **Easy to Use** – Single command, no complex setup.
✅ **Comprehensive Reports** – Detects common vulnerabilities (SSH misconfigurations, open ports, outdated packages, etc.).
✅ **Customizable** – Extendable with your own checks via plugins.
✅ **Open-Source & Free** – No hidden costs, just pure security.

### **Who Is This For?**
- **Developers** – Quickly scan your local environment before deploying.
- **Sysadmins** – Automate security checks in CI/CD pipelines.
- **Security Enthusiasts** – Learn about common vulnerabilities.
- **DevOps Teams** – Integrate into monitoring workflows.

---

## ✨ **Features**

| Feature | Description |
|---------|-------------|
| **🔍 Port Scanning** | Detects open ports and potential services. |
| **🔐 SSH Security Check** | Validates SSH server configurations. |
| **📦 Package Manager Checks** | Warns about outdated packages (APT, YUM, etc.). |
| **🌐 Web Server Checks** | Scans for common HTTP vulnerabilities. |
| **📂 File Permissions Audit** | Checks for overly permissive file/directory settings. |
| **🔄 Plugin System** | Extend functionality with custom checks. |
| **📊 JSON/HTML Reports** | Generate human-readable or machine-parsable reports. |

---

## 🛠️ **Tech Stack**

| Category | Tools/Technologies |
|----------|-------------------|
| **Language** | Bash (v4.0+) |
| **Dependencies** | `nmap`, `ss`, `curl`, `jq` (optional) |
| **Reporting** | JSON, HTML (via `pandoc` or `jq`) |
| **CI/CD** | GitHub Actions (for testing) |
| **Linting** | ShellCheck |

### **System Requirements**
- **Linux/Unix** (macOS with Homebrew may work with adjustments).
- **Bash 4.0+** (most modern systems have this by default).
- **Root/sudo access** (for some checks).

---

## 📦 **Installation**

### **Prerequisites**
Ensure you have the following installed:
```bash
sudo apt update && sudo apt install -y nmap curl jq  # Debian/Ubuntu
sudo yum install -y nmap curl jq                    # RHEL/CentOS
brew install nmap curl jq                            # macOS (Homebrew)
```

### **🚀 Quick Start (Single Command)**
```bash
# Clone the repository
git clone https://github.com/Hemaksh69/Auto-Security-Scanner.git
cd Auto-Security-Scanner

# Make the script executable
chmod +x auto-security-scanner.sh

# Run the scanner (requires sudo for some checks)
sudo ./auto-security-scanner.sh
```

### **📦 Alternative Installation Methods**
#### **1. Install via Package Manager (Coming Soon!)**
We plan to add support for:
- **Homebrew** (`brew install auto-security-scanner`)
- **APT/YUM Repositories**

#### **2. Docker Setup (Experimental)**
```bash
docker build -t auto-security-scanner .
docker run -it --rm -v /:/mnt auto-security-scanner /mnt
```

#### **3. Development Setup**
If you want to contribute:
```bash
git clone https://github.com/yourusername/Auto-Security-Scanner.git
cd Auto-Security-Scanner
make install  # Installs dependencies and symlinks the script
```

---

## 🎯 **Usage**

### **🔍 Basic Usage**
Run the scanner on your local system:
```bash
sudo ./auto-security-scanner.sh
```
**Example Output:**
```json
{
  "status": "completed",
  "timestamp": "2024-05-20T12:34:56Z",
  "checks": {
    "ports": [
      { "port": 22, "service": "ssh", "status": "open", "risk": "medium" }
    ],
    "ssh": {
      "config_valid": true,
      "recommended_changes": ["disable_root_login"]
    },
    "packages": [
      { "name": "nginx", "version": "1.18.0", "outdated": true }
    ]
  }
}
```

### **📊 Generate HTML Report**
```bash
sudo ./auto-security-scanner.sh --format html --output report.html
```
*(Requires `pandoc` for HTML conversion.)*

### **🔄 Custom Checks (Plugin System)**
Add your own checks in the `plugins/` directory. Example:
```bash
# Create a new check (e.g., check_for_weak_passwords.sh)
#!/bin/bash
echo '{"check": "weak_passwords", "status": "found", "details": "User 'root' has empty password"}' | jq .
```
Then run:
```bash
sudo ./auto-security-scanner.sh --plugins plugins/
```

### **🔄 Advanced: CI/CD Integration**
Add this to your `.github/workflows/security.yml`:
```yaml
name: Security Scan
on: [push]
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: |
          git clone https://github.com/yourusername/Auto-Security-Scanner.git
          cd Auto-Security-Scanner
          chmod +x auto-security-scanner.sh
          sudo ./auto-security-scanner.sh --format json > report.json
      - uses: actions/upload-artifact@v3
        with:
          name: security-report
          path: report.json
```

---

## 📁 **Project Structure**
```
Auto-Security-Scanner/
├── auto-security-scanner.sh   # Main script
├── plugins/                   # Custom check plugins
│   └── example_check.sh       # Example plugin
├── templates/                 # Report templates
│   ├── report.json            # JSON template
│   └── report.html            # HTML template
├── tests/                     # Test cases
│   └── test_ssh.sh            # Example test
├── Makefile                   # Build/install scripts
└── README.md                  # You are here!
```

---

## 🔧 **Configuration**

### **Environment Variables**
| Variable | Description | Default |
|----------|-------------|---------|
| `AUTO_SECURITY_SCANNER_THRESHOLD` | Minimum risk level to report (`low`, `medium`, `high`) | `medium` |
| `AUTO_SECURITY_SCANNER_TIMEOUT` | Timeout for checks in seconds | `10` |

**Example:**
```bash
export AUTO_SECURITY_SCANNER_THRESHOLD=high
sudo ./auto-security-scanner.sh
```

### **Customization**
- **Add Checks**: Place new scripts in `plugins/`.
- **Modify Output**: Edit `templates/report.json` or `templates/report.html`.
- **Change Defaults**: Edit `auto-security-scanner.sh` (see `CONFIG` section).

---

## 🤝 **Contributing**

We welcome contributions! Here’s how you can help:

### **🛠️ Development Setup**
1. Fork the repository.
2. Clone locally:
   ```bash
   git clone https://github.com/yourusername/Auto-Security-Scanner.git
   cd Auto-Security-Scanner
   ```
3. Install dependencies:
   ```bash
   make install
   ```
4. Run tests:
   ```bash
   make test
   ```

### **📝 Code Style Guidelines**
- Use **ShellCheck** (`shellcheck auto-security-scanner.sh`).
- Follow **POSIX-compliant Bash** where possible.
- Add **comments** for complex logic.

### **🚀 Submitting a Pull Request**
1. Create a feature branch:
   ```bash
   git checkout -b add-feature-branch
   ```
2. Commit changes:
   ```bash
   git commit -m "Add: New check for Docker vulnerabilities"
   ```
3. Push and open a PR!

### **🎯 Good First Issues**
- [Bug Fixes](https://github.com/yourusername/Auto-Security-Scanner/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
- [Feature Requests](https://github.com/yourusername/Auto-Security-Scanner/issues?q=is%3Aissue+is%3Aopen+label%3A%22enhancement%22)

---

## 📝 **License**

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

---

### **📋 FAQ**
| Question | Answer |
|----------|--------|
| **Does it work on Windows?** | No (Bash is Unix-based). Use WSL or Git Bash. |
| **How often should I run it?** | Weekly for local systems, daily in CI/CD. |
| **Can I run it on remote servers?** | Yes, via SSH: `ssh user@host "sudo ./auto-security-scanner.sh"` |

---

## 🗺️ **Roadmap**

### **🚀 Next Release (v1.2)**
- [ ] Add **Docker security checks**.
- [ ] Support for **Windows Subsystem for Linux (WSL)**.
- [ ] **GUI mode** (using `dialog` or `zenity`).

### **🔮 Long-Term Goals**
- [ ] **Cloud provider integrations** (AWS, GCP, Azure).
- [ ] **Machine learning-based anomaly detection**.
- [ ] **Automated remediation scripts**.


---

## 🎉 **Let’s Make This Better Together!** 🎉

Star ⭐ this repository to show your support! Every star helps us grow the community and improve the tool.


Happy scanning! 🛡️🔍
```

---
### **Key Improvements in This README:**
1. **Professional Badges** – Build status, license, and linting checks.
2. **Engaging Tone** – Encourages contributions and stars.
3. **Clear Structure** – Easy to navigate with collapsible sections (if supported by GitHub).
4. **Practical Examples** – Copy-pasteable commands and usage snippets.
5. **Roadmap & Contribution Guidance** – Attracts developers to help.
6. **Visual Appeal** – Emojis and tables for readability.
7. **CI/CD Integration** – Shows how to use the tool in pipelines.
8. **FAQ & Support** – Reduces repetitive questions.

This README is designed to **convert casual visitors into active contributors** while making the tool **easy to adopt**. 🚀
