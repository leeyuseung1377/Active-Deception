# Active Deception Security System - Progress Update (2026-07-22)

## Overview
This project aims to build an **Active Deception Security System** using Suricata IDS, Nginx Reverse Proxy, and a custom Python-based Risk Engine.

Unlike a traditional IDS that only detects attacks, this system calculates the attacker's risk score and will automatically redirect high-risk attackers to a honeypot.

Current workflow:

Attack
→ Suricata Detection
→ Risk Engine
→ Risk Score
→ SQLite Database
→ (Next) Automatic Honeypot Redirection

---

# Current Architecture

Kali Linux (Attacker)
192.168.85.144

↓

VM1
192.168.85.100

- Nginx Reverse Proxy
- Suricata IDS 8.0.3
- Python Controller
- SQLite Database

↓

Real Web Server
192.168.85.150

OR

Honeypot
192.168.85.160

---

# Environment

## VM1
- Ubuntu
- Nginx Reverse Proxy
- Suricata IDS 8.0.3
- Python Controller
- SQLite

IP:
192.168.85.100

## VM2
Real Web Server

IP:
192.168.85.150

## VM3
Honeypot

IP:
192.168.85.160

## Kali
Attack Machine

IP:
192.168.85.144

---

# Suricata

Version

8.0.3

Monitoring Interface

enp2s0

Rule File

/var/lib/suricata/rules/local.rules

Successfully Loaded

66 Custom Rules

Verification

suricata -T

66 rule(s) successfully loaded
0 failed

---

# OWASP Top 10 (2025) Detection Rules

Implemented custom rules for:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- Directory Traversal
- Sensitive File Access
- SSRF
- XXE
- Brute Force
- Nmap Scan
- Request Smuggling
- Authentication Bypass
- Broken Access Control
- Deserialization

---

# Controller

The Python controller processes Suricata eve.json logs and performs:

- Attack Classification
- Risk Score Calculation
- SQLite Storage
- Risk Aggregation

---

# SQLite Database

Database

/opt/deception/database/deception.db

Tables

alert_events
risk_scores

Stored Information

- attack_type
- signature_id
- signature
- score_added
- timestamp
- repetition_bonus
- diversity_bonus
- burst_bonus

---

# Risk Engine V2

Implemented a multi-factor risk scoring engine.

Final Score

Base Score
+ Signature Bonus
+ Repetition Bonus
+ Diversity Bonus
+ Burst Bonus

---

## Base Score

Severity 1 → +5

Severity 2 → +3

Severity 3 → +1

---

## Signature Bonus

SQL Injection          +4

Directory Traversal    +3

SSRF                   +4

XXE                    +4

XSS                    +2

Command Injection      +5

---

## Repetition Bonus

3 attacks  → +3

5 attacks  → +5

10 attacks → +8

---

## Diversity Bonus

3 different attack types → +5

5 different attack types → +8

---

## Burst Bonus

5 events/minute  → +5

10 events/minute → +10

---

# Attack Classification Improvement

Previous Version

- String-based classification only

Current Version

- SID-based classification (Primary)
- Regex word-boundary string matching (Fallback)

Implemented SID Mapping

9901001 → TRAVERSAL

9901002 → TRAVERSAL

9901006 → SSRF

9901028 → SQL_INJECTION

9901030 → XSS

9901036 → XXE

This approach prevents incorrect classification when rule messages change.

---

# False Positive Fix

Previously

"Unmapped"

↓

Detected as

SCAN

because "nmap" was matched as a substring.

Current Version

Regex word-boundary matching removes this issue.

Verified

"Unmapped test signature"

↓

UNKNOWN

---

# UNKNOWN Handling Improvement

Previous

UNKNOWN alerts could receive

- Repetition Bonus
- Diversity Bonus
- Burst Bonus

resulting in excessively high scores.

Current

UNKNOWN alerts only receive

- Base Score
- Burst Bonus (optional traffic intensity)

This prevents unidentified events from inflating the overall risk score.

---

# Verified Attack Types

Successfully tested

✔ SQL Injection

✔ Directory Traversal

✔ Sensitive File Access

✔ Cross-Site Scripting (XSS)

✔ SSRF

✔ XXE

---

# Latest Verification

SID 9901001

Directory Traversal

↓

TRAVERSAL

SID 9901002

Sensitive Operating System File Access

↓

TRAVERSAL

SID 9901036

XML External Entity Injection

↓

XXE

Example XXE Risk Score

Base Score        5

Signature Bonus   4

Diversity Bonus   5

Final Score      14

Successfully verified.

---

# Current Status

Completed

✔ Infrastructure Setup

✔ Nginx Reverse Proxy

✔ Suricata IDS

✔ 66 OWASP Top 10 Detection Rules

✔ Python Controller

✔ SQLite Integration

✔ Risk Engine V2

✔ SID-based Attack Classification

✔ Regex-based String Classification

✔ False Positive Removal

✔ UNKNOWN Handling Improvement

✔ SQL Injection Detection

✔ Traversal Detection

✔ XSS Detection

✔ SSRF Detection

✔ XXE Detection

✔ Risk Score Verification

---

# Next Phase (V3)

Current

Attack
↓

Detection
↓

Risk Calculation
↓

Database

Next

Attack
↓

Detection
↓

Risk Calculation
↓

Threshold Check
↓

Automatic Nginx Configuration Update
↓

Redirect Attacker to Honeypot

This will complete the Active Deception workflow.

---

# Project Progress

Infrastructure                ██████████ 100%

Suricata Rules                ██████████ 100%

Python Controller             ██████████ 100%

SQLite Integration            ██████████ 100%

Risk Engine V2                ██████████ 100%

Attack Classification         ██████████ 100%

Risk Score Calculation        ██████████ 100%

Detection Verification        ██████████ 100%

Automatic Honeypot Redirect   ███████░░░ 70%

Overall Progress              █████████░ 90%
