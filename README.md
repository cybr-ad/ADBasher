# <div align="center">

![ADBasher Banner](assets/page_header.svg)

# ADBasher

_Automated Active Directory Penetration Testing Framework_

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

</div>

---

## 🚀 NEW: Web Dashboard Available

ADBasher now includes a **modern web interface** for point-and-click penetration testing!

**Quick Start**:

```bash
cd web && chmod +x setup.sh && ./setup.sh
# Choose option 1 (Docker) for fastest setup
# Dashboard available at: http://localhost:3000
```

**Features**:

- ✨ 4-step campaign creation wizard
- 📊 Real-time attack monitoring with WebSocket updates
- 🔍 Live findings feed with auto-refresh
- 📄 One-click report generation
- 🐳 Docker deployment ready

📚 **Full Documentation**: [`web/README.md`](web/README.md) | [`web/QUICKSTART.md`](web/QUICKSTART.md)

---

## 🎯 Overview

**ADBasher** is a comprehensive, unattended Active Directory penetration testing framework designed to automate the complete attack lifecycle—from initial reconnaissance to domain compromise. Built for professional security assessments, ADBasher orchestrates 27+ specialized modules across reconnaissance, credential attacks, post-exploitation, lateral movement, privilege escalation, and persistence phases.

### Key Features

- **🤖 Fully Automated Execution** - Zero user interaction required after launch
- **🔄 Credential Cascading** - Automatically escalates privileges when admin credentials are discovered
- **💾 Database-Driven** - Persistent SQLite storage for all findings and session state
- **📊 Professional Reporting** - Generates both HTML and Markdown reports with executive summaries
- **🛡️ Detection Evasion** - Configurable OpSec modes (Standard/Stealth/Aggressive) with timing jitter
- **🔧 Modular Architecture** - Easy to extend with new attack modules
- **📝 Comprehensive Logging** - Rich console output + JSON logs for SIEM integration

### Attack Lifecycle Coverage

```text
Reconnaissance → Credential Attacks → Post-Exploitation → Lateral Movement → Privilege Escalation → Persistence → Reporting
```

---

## 📋 Table of Contents

- [Features](#features)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Guide](#usage-guide)
- [Configuration](#configuration)
- [Module Reference](#module-reference)
- [Testing](#testing)
- [Contributing](#contributing)
- [Security & Legal](#security--legal)
- [Troubleshooting](#troubleshooting)
- [Documentation](#documentation)
- [License](#license)

---

## ✨ Features

### Core Capabilities

| Phase                    | Modules   | Description                                                                       |
| ------------------------ | --------- | --------------------------------------------------------------------------------- |
| **Reconnaissance**       | 5 modules | DNS discovery, LDAP enumeration, SMB null sessions, network scanning              |
| **Credential Attacks**   | 5 modules | Password spraying, Kerberoasting, AS-REP roasting, admin detection, DCSync checks |
| **Post-Exploitation**    | 4 modules | BloodHound collection, secretsdump, LSASS dumping, DPAPI extraction               |
| **Lateral Movement**     | 1 module  | Multi-method execution (WMI/PSExec/SMBExec/AtExec)                                |
| **Privilege Escalation** | 2 modules | Service misconfigurations, DLL hijacking, delegation abuse                        |
| **Persistence**          | 3 modules | Golden/Silver tickets, ADCS abuse                                                 |
| **Evasion**              | 4 modules | Timing jitter, AMSI bypass, MAC randomization, log cleanup                        |
| **Reporting**            | 2 formats | HTML dashboards + Markdown reports                                                |

### Automation Features

- **Smart Dependencies** - Modules only execute when prerequisites are met
- **Lockout Protection** - Intelligent throttling prevents account lockouts
- **Graceful Degradation** - Continues execution even if individual modules fail
- **Session Management** - All artifacts stored in timestamped session directories
- **Progress Tracking** - Real-time Rich console output with colored status updates

---

## 📁 Repository Structure

```
ADBasher/
├── adbasher.py                 # Main CLI entry point
├── core/                       # Framework core
│   ├── orchestrator.py         # Attack phase orchestration
│   ├── database.py             # SQLite ORM (Targets, Credentials, Vulns)
│   ├── logger.py               # Rich console + JSON logging
│   └── config.yaml             # Global configuration
│
├── 1 nocreds/                  # Reconnaissance (no credentials)
│   ├── discover_domain.py      # DNS SRV record enumeration
│   ├── ldap_anonymous_bind.py  # LDAP anonymous user enumeration
│   ├── smb_null_enum.py        # SMB null session enumeration
│   └── adnetscan_db.py         # Network scanning wrapper
│
├── 3 nopass/automated/         # Credential attacks (valid usernames)
│   ├── password_spray.py       # Lockout-protected password spraying
│   ├── kerberoast.py           # TGS ticket extraction
│   └── asreproast.py           # AS-REP roasting (pre-auth disabled)
│
├── 6 validcreds/automated/     # Post-exploitation (valid credentials)
│   ├── check_admin.py          # Admin privilege detection
│   ├── bloodhound_collect.py   # BloodHound data collection
│   ├── secretsdump_auto.py     # NTDS.dit dumping
│   ├── lsass_dump.py           # LSASS memory dumping
│   ├── dcsync_check.py         # DCSync rights detection
│   ├── dpapi_extract.py        # DPAPI masterkey extraction
│   └── lateral_movement.py     # Multi-method lateral movement
│
├── 7 privesc/automated/        # Privilege escalation
│   ├── privesc_scanner.py      # Service/delegation/registry checks
│   └── dll_hijacking.py        # DLL hijacking opportunity scanner
│
├── 8 persistence/automated/    # Persistence mechanisms
│   ├── golden_ticket.py        # Golden Ticket generation
│   ├── silver_ticket.py        # Silver Ticket generation
│   └── adcs_abuse.py           # Certificate Services exploitation
│
├── evasion/                    # Detection evasion
│   ├── timing.py               # Jitter delays & business hours
│   ├── amsi_bypass.py          # PowerShell AMSI bypass
│   ├── mac_randomization.py    # MAC address randomization
│   └── log_cleanup.py          # Windows Event Log cleanup
│
├── reporting/                  # Report generation
│   └── html_report.py          # Professional HTML report generator
│
├── tests/                      # Test suite
│   ├── test_core.py            # Unit tests (database, logger)
│   ├── test_integration.py     # Integration tests (mock AD)
│   ├── validate_syntax.py      # Python syntax validator
│   └── verify_database.py      # Database integrity checker
│
├── docs/                       # Documentation
│   ├── TESTING.md              # Testing guide & lab setup
│   ├── PERFORMANCE.md          # Optimization guide
│   └── VALIDATION_REPORT.md    # Test results & validation
│
└── resources/                  # Reference materials
    └── pentest_ad_dark_2022_11.svg  # AD attack map
```

---

## 🔧 Prerequisites

### System Requirements

- **OS**: Kali Linux 2024.x, Parrot OS, or Ubuntu 22.04+
- **Python**: 3.10 or higher
- **Privileges**: Root/sudo access (for some modules)
- **Network**: Direct access to target AD environment

### Required Tools

The framework integrates with these external tools (must be installed):

```bash
# Install system packages
sudo apt update
sudo apt install -y crackmapexec impacket-scripts enum4linux-ng

# Optional but recommended
sudo apt install -y bloodhound
pip3 install certipy-ad
```

### Python Dependencies

All Python dependencies are managed via `requirements.txt`:

- `sqlalchemy` - Database ORM
- `pyyaml` - Configuration parsing
- `rich` - Terminal output formatting
- `dnspython` - DNS queries
- `ldap3` - LDAP operations

---

## 📦 Installation

### Method 1: Automated Installation (Recommended)

```bash
# Clone repository
git clone https://github.com/cybr-ad/ADBasher.git
cd ADBasher

# Run automated installer
sudo ./install.sh

# Verify installation
./adbasher.py --help
```

### Method 2: Manual Installation

```bash
# Clone repository
git clone --recurse-submodules https://github.com/cybr-ad/ADBasher.git
cd ADBasher

# Install system dependencies
sudo apt update
sudo apt install -y python3-pip crackmapexec impacket-scripts

# Install Python dependencies
pip3 install -r requirements.txt

# Make executable
chmod +x adbasher.py
chmod +x -R "1 nocreds/" "3 nopass/" "6 validcreds/" "7 privesc/" "8 persistence/"

# Test installation
python3 -c "from core import database, logger, orchestrator; print('✓ Core modules OK')"
```

### Verification

```bash
# Validate all Python modules
python3 tests/validate_syntax.py

# Run unit tests
python3 tests/test_integration.py

# Check tool availability
which crackmapexec secretsdump.py GetUserSPNs.py
```

---

## 🚀 Quick Start

### Basic Usage

```bash
# Target a single domain
./adbasher.py --target example.local

# Target multiple domains/networks
./adbasher.py --target example.local corp.local 192.168.1.0/24

# Use stealth mode (slower but safer)
./adbasher.py --target example.local --opsec stealth
```

### Example Output

```
[14:30:15] ADBasher v1.0 - Automated AD Pentesting Framework
[14:30:15] Session ID: abc12345
[14:30:15] Target: example.local
[14:30:15] OpSec Mode: standard

[Phase 1] Reconnaissance
  -> DNS Discovery: example.local
  ✓ Found DC: DC01.example.local (192.168.1.10)
  -> LDAP Anonymous Bind: 192.168.1.10
  ✓ Enumerated: 250 users

[Phase 2] Credential Attacks
  -> AS-REP Roast: example.local
  ✓ Found 2 vulnerable accounts
  -> Password Spray: 250 users × 7 passwords
  ✓ Valid: EXAMPLE\jdoe:Password123

[Phase 3] Admin Detection
  -> Checking privileges: jdoe
  ✓ Admin access confirmed!

[Phase 4] Post-Exploitation (AUTO-TRIGGERED)
  -> BloodHound Collection
  ✓ Saved: bloodhound_data/20241212_example.zip
  -> Secretsdump: DC01
  ✓ Extracted 250 NTLM hashes

[Phase 5] Lateral Movement
  -> WMIExec: 25 targets
  ✓ Success: 12/25 hosts

[Phase 6] Reporting
  ✓ Report: ~/.adbasher/sessions/abc12345/report.html
  ✓ Report: ~/.adbasher/sessions/abc12345/report.md

[14:45:30] Framework execution complete
[14:45:30] Session artifacts: ~/.adbasher/sessions/abc12345/
```

---

## 📖 Usage Guide

### Command-Line Options

```bash
./adbasher.py [OPTIONS]

Required:
  --target DOMAIN/IP     Target domain(s) or CIDR ranges (space-separated)

Optional:
  --opsec MODE          OpSec mode: standard|stealth|aggressive (default: standard)
  --session-id ID       Resume existing session
  --skip-phases PHASES  Skip specific phases (comma-separated)
  --help                Show help message

Examples:
  ./adbasher.py --target example.local
  ./adbasher.py --target 10.0.0.0/24 example.local --opsec stealth
  ./adbasher.py --target corp.local --skip-phases recon,persistence
```

### Configuration

Edit `core/config.yaml` for advanced settings:

```yaml
global:
  session_dir: ~/.adbasher/sessions
  log_level: INFO

scope:
  target_domains:
    - "example.local"
  exclude_ips:
    - "192.168.1.1" # Gateway

evasion:
  mode: "standard" # standard | stealth | aggressive
  jitter_min: 5 # Minimum delay (seconds)
  jitter_max: 30 # Maximum delay (seconds)
  work_hours_only: false # Only operate 9 AM - 5 PM
```

### Session Management

```bash
# View session artifacts
ls ~/.adbasher/sessions/<SESSION_ID>/

# Key files:
# - session.db           SQLite database
# - session_*.log        Human-readable logs
# - session_*.json.log   SIEM-ready JSON logs
# - report.html          Interactive HTML report
# - report.md            Markdown report
# - bloodhound_data/     BloodHound ZIP files
```

### Database Queries

```bash
# Open session database
sqlite3 ~/.adbasher/sessions/<SESSION_ID>/session.db

# Example queries:
SELECT * FROM targets WHERE is_dc=1;
SELECT username, password FROM credentials WHERE is_admin=1;
SELECT name, severity FROM vulnerabilities;
```

---

## 🧩 Module Reference

### Reconnaissance Modules

| Module                   | Purpose                                | Credentials Required |
| ------------------------ | -------------------------------------- | -------------------- |
| `discover_domain.py`     | DNS SRV record enumeration to find DCs | No                   |
| `ldap_anonymous_bind.py` | LDAP anonymous enumeration for users   | No                   |
| `smb_null_enum.py`       | SMB null session enumeration           | No                   |
| `adnetscan_db.py`        | Network scanning (wraps ADnetscan.sh)  | No                   |

### Credential Attack Modules

| Module              | Purpose                                      | Credentials Required |
| ------------------- | -------------------------------------------- | -------------------- |
| `password_spray.py` | Lockout-protected password spraying          | Usernames only       |
| `kerberoast.py`     | TGS ticket extraction for offline cracking   | Valid domain user    |
| `asreproast.py`     | AS-REP roasting (pre-auth disabled accounts) | No                   |
| `check_admin.py`    | Tests credentials for admin privileges       | Valid credentials    |
| `dcsync_check.py`   | Detects DCSync rights                        | Valid credentials    |

### Post-Exploitation Modules

| Module                  | Purpose                    | Credentials Required |
| ----------------------- | -------------------------- | -------------------- |
| `bloodhound_collect.py` | BloodHound data collection | Valid domain user    |
| `secretsdump_auto.py`   | NTDS.dit dumping           | Domain Admin         |
| `lsass_dump.py`         | LSASS memory dumping       | Local Admin          |
| `dpapi_extract.py`      | DPAPI masterkey extraction | Valid user           |

### Advanced Modules

See `docs/` directory for detailed module documentation.

---

## 🧪 Testing

### Automated Tests

```bash
# Run all tests
cd ADBasher

# 1. Syntax validation (107 Python files)
python3 tests/validate_syntax.py

# 2. Unit tests (database, logger, config)
python3 tests/test_core.py

# 3. Integration tests (mock AD environment)
python3 tests/test_integration.py

# 4. Database integrity check (after running framework)
python3 tests/verify_database.py ~/.adbasher/sessions/<SESSION_ID>/session.db
```

### Lab Environment Testing

For comprehensive testing, deploy a lab Active Directory environment:

```bash
# See detailed lab setup guide
cat docs/TESTING.md

# Recommended: GOAD (Game of Active Directory)
# Or DetectionLab for testing with logging/monitoring
```

**Test Coverage**: 85% automated, 95% with manual lab testing

---

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

### Development Setup

```bash
# Fork and clone
git clone https://github.com/cybr-ad/ADBasher.git
cd ADBasher

# Create feature branch
git checkout -b feature/your-feature-name

# Install development dependencies
pip3 install -r requirements-dev.txt  # (if available)
```

### Adding New Modules

1. **Create module file**: Place in appropriate phase directory (`1 nocreds/`, `3 nopass/`, etc.)
2. **Follow template**: Use existing modules as reference
3. **Database integration**: Use `DatabaseManager` for persistence
4. **Logging**: Import and use framework logger
5. **CLI arguments**: Use argparse with `--session-dir` parameter
6. **Test**: Add unit tests to `tests/test_core.py`

Example module template:

```python
#!/usr/bin/env python3
import sys
import os
import argparse

sys.path.append(os.path.abspath(os.path.join(os.path.dirname(__file__), '../..')))

from core.database import DatabaseManager
from core.logger import setup_logger, get_logger

logger = None

def your_module_function(session_dir, target, **kwargs):
    global logger
    setup_logger("your_module", session_dir)
    logger = get_logger("your_module")

    db_path = os.path.join(session_dir, "session.db")
    db = DatabaseManager(db_path)

    # Your attack logic here
    logger.info(f"Starting attack against {target}")

    # Store results
    db.add_target(ip=target, hostname="example")

    logger.info("Attack complete")

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--session-dir", required=True)
    parser.add_argument("--target", required=True)
    args = parser.parse_args()

    your_module_function(args.session_dir, args.target)
```

### Code Standards

- **Python**: PEP 8 compliant
- **Docstrings**: Google-style docstrings for all functions
- **Type hints**: Use where applicable
- **Error handling**: Try-except blocks with proper logging
- **Testing**: Add tests for new functionality

### Pull Request Process

1. Update documentation (README.md, module docstrings)
2. Run automated tests: `python3 tests/validate_syntax.py`
3. Test in lab environment
4. Submit PR with clear description of changes

---

## 🔒 Security & Legal

### ⚠️ WARNING

**This tool performs ACTIVE ATTACKS against systems.**

- ✅ **ONLY** use on systems you own or have **written authorization** to test
- ✅ Obtain **signed scope agreements** before engagements
- ✅ Follow **local laws** and **industry regulations** (GDPR, HIPAA, PCI-DSS)
- ❌ **Unauthorized access is a CRIME** in most jurisdictions

### Responsible Use

- **Penetration Testing**: Authorized security assessments only
- **Red Team Exercises**: With proper client authorization
- **Security Research**: In isolated lab environments
- **Education**: Personal learning in controlled environments

### Disclaimer

The authors and contributors are not responsible for misuse of this tool. Users are solely responsible for ensuring they have proper authorization before using ADBasher.

---

## 🐛 Troubleshooting

### Common Issues

#### 1. "Module not found" errors

```bash
# Solution: Set PYTHONPATH
export PYTHONPATH=/path/to/ADBasher:$PYTHONPATH
./adbasher.py --target example.local

# Or use absolute path
python3 /full/path/to/adbasher.py --target example.local
```

#### 2. "Permission denied" on tools

```bash
# Solution: Make scripts executable
chmod +x adbasher.py
chmod +x -R "1 nocreds/" "3 nopass/" "6 validcreds/"
```

#### 3. Database locked errors

```bash
# Solution: Kill lingering processes
pkill -f adbasher
rm ~/.adbasher/sessions/*/session.db-journal  # if exists
```

#### 4. Missing tool dependencies

```bash
# Solution: Install missing tools
sudo apt install crackmapexec impacket-scripts
pip3 install bloodhound certipy-ad
```

### Debug Mode

```bash
# Enable verbose logging
# Edit core/config.yaml:
global:
  log_level: DEBUG  # Change from INFO

# Then run normally
./adbasher.py --target example.local
```

### Getting Help

1. **Check documentation**: `docs/` directory
2. **Search issues**: GitHub Issues tab
3. **Review logs**: `~/.adbasher/sessions/<SESSION_ID>/session_*.log`
4. **Submit bug report**: Include logs and error messages

---

## 📚 Documentation

### Documentation Index

| Document                                                                 | Description                               |
| ------------------------------------------------------------------------ | ----------------------------------------- |
| [TESTING.md](docs/TESTING.md)                                            | Lab setup, test procedures, validation    |
| [PERFORMANCE.md](docs/PERFORMANCE.md)                                    | Optimization guide, profiling, benchmarks |
| [VALIDATION_REPORT.md](docs/VALIDATION_REPORT.md)                        | Test results, coverage metrics            |
| [Walkthrough](https://github.com/cybr-ad/ADBasher/wiki/Walkthrough) | Step-by-step execution example            |

### External Resources

- [Orange Cyberdefense AD Mind Map](resources/pentest_ad_dark_2022_11.svg)
- [MITRE ATT&CK - Active Directory](https://attack.mitre.org/)
- [BloodHound Documentation](https://bloodhound.readthedocs.io/)
- [Impacket Examples](https://github.com/fortra/impacket/tree/master/examples)

---

## 📊 Project Statistics

- **Total Modules**: 27 Python modules
- **Lines of Code**: ~9,000+
- **Test Coverage**: 85%
- **Supported Tools**: 10+ (Impacket, CrackMapExec, BloodHound, etc.)
- **Attack Techniques**: 40+
- **Python Files**: 107 (including dependencies)

---

## 🗺️ Roadmap

### V1.1 (Planned)

- [ ] Multi-threading for parallel scanning
- [ ] Automated hash cracking (Hashcat integration)
- [ ] Neo4j integration for BloodHound analysis
- [ ] Web dashboard (Flask)
- [ ] Azure AD enumeration support

### V2.0 (Future)

- [ ] C2 integration (Cobalt Strike, Metasploit)
- [ ] Docker containerization
- [ ] Distributed scanning
- [ ] AI-powered attack path selection
- [ ] MITRE ATT&CK mapping

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 👥 Authors & Acknowledgments

### Primary Authors

- **Project Lead**: [Your Name]
- **Contributors**: See [CONTRIBUTORS.md](CONTRIBUTORS.md)

### Acknowledgments

- **Orange Cyberdefense** - AD penetration testing methodology
- **BloodHound Team** - Attack path enumeration inspiration
- **Impacket Developers** - Protocol implementations
- **Open Source Community** - Tool integrations and libraries

### Tools Integrated

- [Impacket](https://github.com/fortra/impacket)
- [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)
- [BloodHound](https://github.com/BloodHoundAD/BloodHound)
- [Certipy](https://github.com/ly4k/Certipy)

---

## 📞 Contact & Support

- **GitHub Issues**: [Report bugs or request features](https://github.com/cybr-ad/ADBasher/issues)
- **Discussions**: [Ask questions](https://github.com/cybr-ad/ADBasher/discussions)
- **Email**: <security@example.com> (for security disclosures)

---

## 🌟 Star History

If you find ADBasher useful, please consider starring the repository! ⭐

---

<div align="center">
  
**Made with ❤️ by security professionals, for security professionals**

[Documentation](docs/) • [Contributing](CONTRIBUTING.md) • [License](LICENSE) • [Changelog](CHANGELOG.md)

</div>
