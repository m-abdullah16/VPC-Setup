# VPC-Setup
# 🏰 VPC-Setup — I Built a Digital Fortress

Turns out "networking" isn't scary once you've locked yourself out of your own server three times trying to fix it. This is that story, in repo form.

![Built with](https://img.shields.io/badge/built%20with-AWS%20VPC-FF9900?logo=amazonaws&logoColor=white)
![Status](https://img.shields.io/badge/status-live%20%26%20verified-3ecf8e)
![Made by](https://img.shields.io/badge/built%20by-Abdullah-4d9fff)

## 🤔 What is this?

A real, hand-built 2-tier VPC on AWS — a public subnet out front, a private subnet locked away in the back, and exactly one guarded door between them.

Think of it like a house: the public subnet is the porch — anyone can knock. The private subnet is the vault in the basement — nobody gets in unless they come through the front door first, and even then, only if they're on the list.

## 🧱 The Architecture

- VPC: 172.31.0.0/16 — the whole plot of land (65,536 addresses of it)
- Public subnet: 172.31.0.0/20 (ap-south-1a) — home of the bastion host, the one instance allowed to talk to the outside world directly
- Private subnet: 172.31.48.0/20 (ap-south-1b) — the vault. No public IP. No direct route in. Ever.
- Internet Gateway — the front gate, attached at the VPC level
- NAT Gateway — sits in the public subnet so the private instance can still phone out (updates, patches) without ever accepting calls in
- Dedicated route tables per subnet — public and private each get their own rulebook, so nothing accidentally inherits internet access it shouldn't have

## 🔑 Why It's Built This Way

The private subnet has zero route to the Internet Gateway — it doesn't exist to the outside internet at all. The only way in is a deliberate hop: you → bastion host → private instance, one exposed door instead of two.

The NAT Gateway solves a real problem: the private instance still needs to run updates sometimes, but it should never be reachable from outside. NAT gives it outbound-only internet — it can ask, but nothing can knock.

Splitting across an Availability Zone setup also means a single data-center hiccup doesn't take the whole thing down — small setup now, but the same principle that keeps real production systems standing.

## ✅ Verification — Not Just "It Should Work"

- [x] Public instance reachable via SSH, confirmed outbound internet access
- [x] Private instance confirmed to have no public IP
- [x] Private instance reached successfully via bastion-host hop
- [x] Private instance confirmed outbound-only internet via NAT Gateway
- [x] Survived multiple "why can't I connect" debugging sessions and came out the other side actually understanding CIDR math

## 🗺️ Diagram

<img width="3063" height="1378" alt="MAP2" src="https://github.com/user-attachments/assets/455f511d-a2d9-4ecc-9b22-29d57eeadcb5" />
<img width="3063" height="1378" alt="MAP1" src="https://github.com/user-attachments/assets/29410e33-0ca6-4feb-9642-342ad8deb111" />

## 🚧 What's Next

- [ ] Move Security Group rules to Infrastructure as Code (Terraform, Month 2 territory)
- [ ] Add a second AZ for genuine high availability, not just theoretical
- [ ] Swap manual bastion SSH for AWS Systems Manager Session Manager — zero open ports, zero drama

---

⭐ If watching someone debug their way through subnet CIDR math in real time sounds like a good time, you'll like the commit history.

