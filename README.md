👋 Hi, I'm fmabella

About Me

My path into cybersecurity didn't start with a textbook — it started with a conversation. Someone close to me, John Doe, introduced me to the world of security and showed me just how much there is to learn and protect. That spark turned into a passion, and I've been building my skills ever since. I'm currently focused on blue team operations and SOC work, with hands-on experience in threat detection, log analysis, and web application security. I believe the best defenders are the ones who understand how attackers think — and CTF competitions have been one of my favorite ways to develop that mindset.

🚩 CTF Experience — National Cyberleague 2026

Competing in the 2026 National Cyberleague as an all-around player was one of the most challenging and rewarding experiences of my cybersecurity journey so far. Every challenge pushed me to think critically under pressure, dig deeper into tools I thought I knew, and pick up entirely new techniques on the fly. The competition reinforced something important: cybersecurity isn't just about knowing the right answer — it's about knowing how to find it.

📊 Scouting Reports

📄 Individual Scouting Report (https://s3.us-east-1.amazonaws.com/user-data.production.cyberskyline.com/reports/66e8dc5411ff08a1109ee522/DX67F6QLUTW2.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAUDF4ZW3OYCTQP3HR%2F20260508%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260508T043233Z&X-Amz-Expires=600&X-Amz-Signature=d551ab6a1a8fd9c6fd6a3f32a8d225c20c42a3ed6a22103a0a29cba7f266a9a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

👥 Team Scouting Report (https://s3.us-east-1.amazonaws.com/user-data.production.cyberskyline.com/reports/66e8dc5411ff08a1109ee522/9JNRJXK4GWFB.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAUDF4ZW3OYCTQP3HR%2F20260508%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260508T043152Z&X-Amz-Expires=600&X-Amz-Signature=31f3eab613b3dbf958c3704212870cbf6d0d7e2480248c06dbba6bc3846aeddb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

# CTF-Tools-and-Tips
# 🛠️ CTF Tools & Tips

A personal reference guide built from hands-on CTF experience, This document covers the tools I rely on most, organized by challenge category, along with tips and workflows that have saved me time during competition.

> ⚠️ All techniques described here are for educational purposes and ethical, authorized use only.

---

## 📋 Table of Contents

- [General CTF Mindset](#-general-ctf-mindset)
- [CyberChef](#-cyberchef)
- [Web Application Penetration Testing](#-web-application-penetration-testing)
- [IDOR](#-idor-insecure-direct-object-reference)
- [Password Cracking](#-password-cracking)
- [Network Traffic Analysis](#-network-traffic-analysis)
- [Forensics](#-forensics)
- [General Tips](#-general-tips)

---

## 🧠 General CTF Mindset

- **Read everything twice.** Challenge descriptions almost always contain a hint.
- **Don't tunnel vision.** If you've been stuck for 20+ minutes, switch challenges and come back fresh.
- **Document as you go.** Take notes on every attempt — you'll thank yourself later when writing up or backtracking.
- **Know your category.** Each CTF domain has a standard toolkit. Build muscle memory with your tools before competition day.

---

## 🔧 CyberChef

[CyberChef] is the Swiss Army knife of data transformation. It's browser-based, fast, and incredibly powerful for encoding, decoding, and encryption challenges.

### Key Operations to Know
| Operation | Use Case |
|---|---|
| `From Base64` | Decode Base64-encoded strings |
| `From Hex` | Convert hex dumps to readable text |
| `ROT13` | Quick Caesar cipher check |
| `XOR` | Decrypt XOR-obfuscated data |
| `Magic` | Auto-detect encoding (great starting point) |
| `Gunzip / Inflate` | Decompress compressed data |
| `To Charcode` | Convert characters to ASCII codes |
| `Regular Expression` | Extract patterns from messy output |

### Tips
- **Use Magic first** when you're not sure what encoding you're dealing with — it'll auto-detect and suggest operations.
- **Chain operations** using the recipe builder. Many CTF flags are encoded in multiple layers (e.g., Base64 → XOR → Hex).
- **Save your recipes** using the bookmark feature so you don't rebuild common chains mid-competition.
- Look for `=` padding at the end of a string — that's almost always Base64.

---

## 🌐 Web Application Penetration Testing

Web challenges test your ability to find and exploit vulnerabilities in intentionally misconfigured applications.

### Core Tools
| Tool | Purpose |
|---|---|
| **Burp Suite** | Intercept, modify, and replay HTTP requests |
| **browser DevTools** | Inspect source, cookies, headers, JS |
| **curl** | Send custom HTTP requests from the terminal |
| **Gobuster / ffuf** | Directory and endpoint fuzzing |
| **Wappalyzer** | Identify tech stack of a target app |

### Common Vulnerability Types in CTFs
- **SQL Injection** — Try `' OR 1=1--` in login fields; use `sqlmap` for automation
- **XSS (Cross-Site Scripting)** — Test input fields with `<script>alert(1)</script>`
- **Command Injection** — Look for fields that interact with the OS; test with `;`, `|`, `&&`
- **Directory Traversal** — Try `../../etc/passwd` in file path parameters
- **Broken Authentication** — Check for weak session tokens, predictable cookies, or missing auth checks

### Tips
- **Always check page source first** — flags and hints are sometimes hidden in HTML comments.
- **Intercept everything with Burp Suite.** Many CTF web challenges require modifying headers, cookies, or POST body parameters.
- **Fuzz directories early** — hidden endpoints like `/admin`, `/backup`, or `/api` are common in CTF web challenges.
- Don't forget to check `robots.txt` and `.git` directories — they're classic CTF hiding spots.

---

## 🔓 IDOR (Insecure Direct Object Reference)

IDOR vulnerabilities occur when an application exposes internal object references (like user IDs or file names) that can be manipulated to access unauthorized data.

### How to Identify IDOR
- Look for numeric or sequential IDs in URLs, POST bodies, or cookies
  - Example: `https://example.com/profile?user_id=1042`
- Check for object references in API responses
- Watch for filenames passed as parameters: `?file=report_user1042.pdf`

### How to Exploit IDOR
1. Capture the request in Burp Suite
2. Identify the object reference (user ID, account number, filename)
3. Modify the value to reference another object (e.g., change `1042` → `1001`)
4. Replay the request and observe the response

### Tips
- **Try IDs 1, 2, 3 first** — admin accounts are often the lowest-numbered users.
- **Don't just look in the URL** — object references hide in POST bodies, JSON payloads, and cookies too.
- **Automate enumeration** with Burp Intruder or a simple Python script when IDs are numeric.
- If IDs are UUIDs (non-sequential), look for them leaking in other API responses or page source.

---

## 🔑 Password Cracking

Password cracking challenges provide a hash and ask you to recover the original plaintext.

### Identify the Hash First
Use online hash identifiers to determine the hash type before cracking.

| Hash Format | Example |
|---|---|
| MD5 | `5f4dcc3b5aa765d61d8327deb882cf99` |
| SHA-1 | `5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8` |
| SHA-256 | `64-character hex string` |
| bcrypt | Starts with `$2b$` or `$2a$` |
| NTLM | Windows credential hash |

### Core Tools
| Tool | Best For |
|---|---|
| **Hashcat** | GPU-accelerated cracking, large wordlists |
| **John the Ripper** | Versatile, good for quick local cracking |
| **CrackStation** | Online lookup for common hashes |
| **hashes.com** | Online reverse hash lookup |

### Common Hashcat Attack Modes
```bash
# Wordlist attack (most common starting point)
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt

# Wordlist + rules (mutates words with common substitutions)
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt -r best64.rule

# Mask attack (brute force with known pattern, e.g., 8-char alphanumeric)
hashcat -m 0 hash.txt -a 3 ?a?a?a?a?a?a?a?a

# Check cracked results
hashcat -m 0 hash.txt --show
```

### Tips
- **Always try rockyou.txt first** — it covers a huge percentage of CTF passwords.
- **Apply rules before going to brute force.** The `best64.rule` ruleset catches common substitutions (e.g., `password` → `P@ssw0rd`).
- **Use online lookups for MD5/SHA-1** — CrackStation and hashes.com often have the answer instantly.
- If the hash starts with `$2b$` (bcrypt), it'll be slow — budget extra time and try a smaller, targeted wordlist.
- Don't forget to check if the "hash" is just encoded (Base64, hex) rather than truly hashed — decode it in CyberChef first.

---

## 📡 Network Traffic Analysis

### Core Tools
| Tool | Purpose |
|---|---|
| **Wireshark** | Full packet capture analysis |
| **tshark** | CLI version of Wireshark |
| **NetworkMiner** | Extract files and credentials from pcaps |

### Tips
- Use `Follow TCP Stream` in Wireshark to reconstruct full conversations.
- Filter by protocol: `http`, `ftp`, `dns`, `smtp` to narrow down traffic quickly.
- Export HTTP objects (`File > Export Objects > HTTP`) to recover transferred files.
- Credentials sent over unencrypted protocols (FTP, HTTP, Telnet) are often hiding in pcap challenges.

---

## 🔍 Forensics

### Core Tools
| Tool | Purpose |
|---|---|
| **Autopsy / FTK Imager** | Disk image analysis |
| **Binwalk** | Extract embedded files from binaries |
| **Steghide / zsteg** | Detect and extract steganography |
| **ExifTool** | Read metadata from images/files |
| **Strings** | Extract readable strings from binaries |
| **file** | Identify true file type regardless of extension |

### Tips
- **Always run `file` and `strings` first** on any unknown binary or image.
- **Check file headers (magic bytes)** — a `.jpg` with a `PK` header is actually a ZIP file.
- Run `binwalk -e` on suspicious files to extract hidden embedded content.
- Image challenges almost always involve steganography — try `steghide extract -sf image.jpg` with a blank passphrase first.

---

## 💡 General Tips

- **Flag format matters** — know your competition's flag format (e.g., `FLAG{...}`) so you recognize it instantly.
- **Google is your friend.** CTF challenges often reference real CVEs, tools, or techniques — a quick search can unlock the whole challenge.
- **Keep a personal cheat sheet.** Build it challenge by challenge. Your future self will thank you.
- **Communicate with your team.** Share partial progress — sometimes a fresh set of eyes solves it in minutes.
- **Time-box yourself.** Spending 2 hours on one challenge while skipping easy points elsewhere is a common mistake.
- **Write everything down.** Even failed attempts have value — for write-ups, for learning, and for not repeating yourself.



*Built from real CTF competition experience. Contributions and suggestions welcome!*
