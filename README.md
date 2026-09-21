## Project Write-up

This project is documented step by step:

1. [Network Discovery](docs/01-network-discovery.md)
2. [Port and Service Scanning](docs/02-port-and-service-scanning.md)
3. [Findings and Recommendations](docs/03-findings-and-recommendations.md)

## Key Findings

- **Medium** — Router's web admin interface reachable over unencrypted HTTP with no HTTPS available, exposing login credentials in plain text to anyone already on the network.
- **Low/Informational** — Initial host discovery scan overreported live devices (8 vs. 5 actual), due to one device appearing under multiple IPs and the scanning machine itself being counted.

Full details, evidence, and remediation in [Findings and Recommendations](docs/03-findings-and-recommendations.md).

# Home Network Vulnerability Scan

A security assessment of my house network, carried out to practice identifying and reporting on common network misconfigurations using open-source tools before persuing a role in Cybersecurity.

## Why I did this

Technology is advancing every day, and so is the need to protect it, that's
what drew me to cybersecurity. Nmap is a tool used by real security
professionals for network reconnaissance, and I wanted to learn on something
real rather than a toy exercise. I wanted practical, evidence-based experience
with vulnerability scanning rather than just theory, so I examined a network
I have permission to test, documented each step, and turned the results into
a set of prioritized fixes.

## Scope

This assessment was performed only against my home network and devices I own.
No external or third-party systems were scanned. Scanning networks without
authorization is a criminal offence in the UK under the Computer Misuse Act
1990 — this project was scoped to stay clearly inside that line.
