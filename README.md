# VPC-Setup
# Secure 2-Tier VPC Architecture on AWS

## Overview
A production-style VPC with public and private subnet
tiers, built to isolate backend resources from direct
internet exposure while still allowing outbound updates.

## Architecture
- VPC: 10.0.0.0/16
- Public subnet: 10.0.0.0/24 (AZ-a) — web-facing tier
- Private subnet: 10.0.1.0/24 (AZ-b) — isolated tier
- Internet Gateway attached at VPC level
- NAT Gateway deployed in public subnet for
  outbound-only private subnet access
- Dedicated route tables per subnet (no shared
  main-table exposure risk)

## Why this design
Explain the public/private separation, why NAT instead
of exposing the private tier, and the AZ split for
high availability.

## Verification performed
- Public instance reachable via SSH, confirmed
  internet access
- Private instance confirmed to have no public IP
- Private instance confirmed outbound-only access
  via bastion-host hop + NAT Gateway

## Diagram
<img width="3063" height="1378" alt="MAP2" src="https://github.com/user-attachments/assets/455f511d-a2d9-4ecc-9b22-29d57eeadcb5" />
<img width="3063" height="1378" alt="MAP1" src="https://github.com/user-attachments/assets/29410e33-0ca6-4feb-9642-342ad8deb111" />

