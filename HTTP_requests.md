# 📡 HTTP Requests — Complete Notes for Quick Reference

> **Module:** API Fundamentals → HTTP Requests  
> **Date:** February 2026  
> **Source:** Codecademy

---

## 📌 Table of Contents
1. [What is HTTP?](#1-what-is-http)
2. [Key Terms Glossary](#2-key-terms-glossary)
3. [How a Web Request Works (Step-by-Step)](#3-how-a-web-request-works-step-by-step)
4. [HTTP Methods](#4-http-methods)
5. [HTTP Status Codes](#5-http-status-codes)
6. [HTTP/1.0 vs HTTP/1.1](#6-http10-vs-http11)
7. [TCP 3-Way Handshake](#7-tcp-3-way-handshake)
8. [HTTPS — The Secure Version](#8-https--the-secure-version)
9. [Evolution of HTTP](#9-evolution-of-http)
10. [Master Analogy (The Town)](#10-master-analogy-the-town)
11. [Memory Tricks Cheatsheet](#11-memory-tricks-cheatsheet)

---

## 1. What is HTTP?

**HTTP = HyperText Transfer Protocol**

- A set of **rules (protocol)** that defines how a browser (client) and a website (server) communicate.
- Think of it as a **shared language** — both sides must speak it to understand each other.
- Data travels physically over the internet using **TCP**.

```
Browser (Client)  ←——— HTTP ———→  Server
```

> 🧠 **Memory Trick:** HTTP = **H**ow **T**o **T**alk to a **P**age

---

## 2. Key Terms Glossary

| Term | Definition | Quick Memory Hook |
|------|-----------|------------------|
| **Client** | Your browser — the one *asking* for content | *Client = Customer* |
| **Server** | Remote computer that *stores & sends* content | *Server = Waiter* |
| **Protocol** | A set of agreed-upon rules both sides follow | *Protocol = Rulebook* |
| **URL** | Web address (e.g. `www.codecademy.com`) | *URL = Street Address* |
| **IP Address** | Numeric location of a server (e.g. `151.101.1.195`) | *IP = GPS Coordinates* |
| **DNS** | Converts URL names → IP addresses | *DNS = Internet Phone Book* |
| **TCP** | Protocol that physically moves data reliably | *TCP = The Train Tracks* |
| **Resource** | Any file on the web: HTML, CSS, image, video, JS | *Resource = Any item in a store* |
| **HTTP Method** | Type of action being requested (GET, POST, etc.) | *Method = Type of Order* |
| **Status Code** | Server's 3-digit reply about what happened | *Status Code = Order Receipt* |

---

## 3. How a Web Request Works (Step-by-Step)

### When you type `www.codecademy.com` and hit Enter:

```
Step 1: Browser reads "http://" → knows which protocol to use

Step 2: DNS Lookup
        "codecademy.com" ──→ DNS ──→ "151.101.1.195"
        (name)                         (IP address)

Step 3: TCP Connection (3-way handshake)
        Browser ──→ Server  "Can we connect?"
        Server  ──→ Browser "Yes! You ready?"
        Browser ──→ Server  "Ready. Let's go!"

Step 4: Browser sends HTTP GET Request
        GET / HTTP/1.1
        Host: www.codecademy.com

Step 5: Server responds
        HTTP/1.1 200 OK
        Content-Type: text/html
        [webpage content here]

Step 6: TCP connection closes (HTTP/1.0) or stays open (HTTP/1.1)
```

> 🏘️ **Analogy:** You write a letter in HTTP language → mailman builds train tracks (TCP) → rides to the business → business fulfills or rejects your order → mailman returns with the result.

---

## 4. HTTP Methods

| Method | Purpose | Analogy |
|--------|---------|---------|
| `GET` | **Retrieve** data from server | *Reading* a menu |
| `POST` | **Send** new data to server | *Placing* a new order |
| `PUT` | **Update** existing data on server | *Changing* your order |
| `DELETE` | **Remove** data from server | *Cancelling* your order |

### Example GET Request:
```http
GET / HTTP/1.1
Host: www.codecademy.com
```
- `GET` → method type
- `/` → path (root/homepage)
- `HTTP/1.1` → version of protocol
- `Host` → which server to talk to

> 🧠 **Memory Trick — CRUD:**  
> **C**reate = POST | **R**ead = GET | **U**pdate = PUT | **D**elete = DELETE

---

## 5. HTTP Status Codes

Status codes are **3-digit numbers** the server sends back to describe what happened.

### Categories at a Glance:
```
1xx → Informational   (hold on...)
2xx → Success         (here you go!)
3xx → Redirection     (it moved, follow me...)
4xx → Client Error    (you did something wrong)
5xx → Server Error    (we messed up)
```

### Most Important Ones:

| Code | Name | Meaning | Analogy |
|------|------|---------|---------|
| `200` | OK | ✅ Request succeeded | *"Here's your order!"* |
| `301` | Moved Permanently | Page has a new URL forever | *"We moved. New address: ..."* |
| `404` | Not Found | ❌ Page doesn't exist | *"We don't carry that item."* |
| `500` | Internal Server Error | 💥 Server crashed/failed | *"Our kitchen is on fire."* |

> 🧠 **Memory Trick:**  
> - **2**xx = ✅ **2** thumbs up (success)  
> - **4**xx = ❌ **4**ault of the client (you messed up)  
> - **5**xx = 💥 **5**erver is broken (their fault)

---

## 6. HTTP/1.0 vs HTTP/1.1

### The Problem:
A single webpage = **many separate files**:
```
index.html + style.css + logo.png + banner.jpg + app.js = 5 files
```

### HTTP/1.0 — One File = One Full Connection
```
OPEN connection → GET index.html   → CLOSE
OPEN connection → GET style.css    → CLOSE
OPEN connection → GET logo.png     → CLOSE
... (repeated for EVERY file)
```
❌ Slow — every file requires a full connection ceremony.

### HTTP/1.1 — One Connection, Many Files (Persistent)
```
OPEN connection
  → GET index.html   ✅
  → GET style.css    ✅
  → GET logo.png     ✅
  → GET app.js       ✅
CLOSE connection
```
✅ Fast — connection stays open and is reused.

### Side-by-Side:

| | HTTP/1.0 | HTTP/1.1 |
|--|---------|---------|
| **Connections per page** | One per file | One per page |
| **Speed** | 🐢 Slower | ⚡ Faster |
| **TCP Handshakes** | Many | One |
| **Released** | 1996 | 1997 |

> 🛒 **Analogy:**  
> - **HTTP/1.0** = Leave the store and re-enter for every single grocery item.  
> - **HTTP/1.1** = Grab a cart, get ALL items in one trip, pay once, leave.

> 🧠 **Memory Trick:** 1.1 = "**1** trip for **1** page, no matter how many files."

---

## 7. TCP 3-Way Handshake

Before any HTTP data is sent, TCP requires a brief **"greeting ceremony"**:

```
Client → Server:  "SYN"      ("Hello, can we connect?")
Server → Client:  "SYN-ACK"  ("Yes! Can you hear me?")
Client → Server:  "ACK"      ("Heard you. Let's go!")
         ↓
    Data transfer begins
```

This adds a small time delay. HTTP/1.0 does this for **every file**. HTTP/1.1 does it **once per page**.

> 📞 **Analogy:** It's like being forced to say *"Hello? Is this John? This is Sarah, can you hear me?"* before every single sentence of a phone call — rather than just having one continuous conversation.

> 🧠 **Memory Trick:** **SYN** = "**Syn**chronize" (let's get in sync) | **ACK** = "**Ack**nowledge" (got it!)

---

## 8. HTTPS — The Secure Version

**HTTPS = HTTP + Secure (Encrypted)**

| | HTTP | HTTPS |
|-|------|-------|
| **Data** | Plain text (anyone can read) | Encrypted (scrambled) |
| **URL starts with** | `http://` | `https://` |
| **Safe for passwords?** | ❌ No | ✅ Yes |
| **Requires certificate?** | No | Yes (from a CA) |

### How it works:
```
Without HTTPS:
You → [password: abc123] → 🕵️ (anyone can intercept and read this)

With HTTPS:
You → [x7#kP!q9] → 🔒 (encrypted, unreadable to snoopers) → Server
```

### Certificate Authority (CA):
- A trusted organization that **verifies** a website is legitimate.
- Like an official ID card — proves the server is who it says it is.
- Examples: Let's Encrypt, DigiCert, Comodo.

> 📬 **Analogy:**  
> - **HTTP** = Sending a postcard — anyone who handles it can read it.  
> - **HTTPS** = Sending a sealed, padlocked envelope — only sender and receiver can open it.

> 🧠 **Memory Trick:** The **S** in HTTPS = **S**afe, **S**ecure, **S**ealed.

---

## 9. Evolution of HTTP

```
HTTP/1.0  (1996) → One connection per file            🐢 Slow
     ↓
HTTP/1.1  (1997) → Reuse one connection per page      ⚡ Faster
     ↓
HTTP/2    (2015) → Multiple files sent in parallel     🚀 Much Faster
     ↓
HTTP/3    (2022) → Built on UDP instead of TCP         🛸 Blazing Fast
```

> 🧠 **Memory Trick:** Each version solved the bottleneck of the previous one — think of it as upgrading from a footpath → single road → multi-lane highway → teleportation.

---

## 10. Master Analogy (The Town)

| Internet Concept | Town Analogy |
|-----------------|-------------|
| You (the browser) | A town resident writing letters |
| HTTP | The language written in the letter |
| TCP | The train tracks connecting you to businesses |
| DNS | The town's phone/address book |
| Server | A business that fulfills requests |
| GET Request | Your written order |
| IP Address | The exact street address of the business |
| `200 OK` | *"Here's exactly what you asked for!"* |
| `404 Not Found` | *"We don't have that here."* |
| `500 Error` | *"Our store is currently on fire."* |
| HTTP/1.0 | Tear down and rebuild tracks after every delivery |
| HTTP/1.1 | Keep the tracks up for the whole shopping trip |
| HTTPS | Sending a sealed, locked envelope instead of a postcard |

---

## 11. Memory Tricks Cheatsheet

| Concept | Memory Trick |
|---------|-------------|
| **HTTP** | **H**ow **T**o **T**alk to a **P**age |
| **TCP** | The **T**rain tra**C**ks that carry **P**ackages |
| **DNS** | The internet's **D**irectory/**N**ame-to-address **S**ystem |
| **URL** | Your destination's **U**nique **R**oad **L**ocation |
| **GET** | GET = *"Give me"* — you're retrieving something |
| **POST** | POST = *"Here's something new"* — you're sending data |
| **PUT** | PUT = *"Replace this"* — updating existing data |
| **DELETE** | DELETE = *"Throw it away"* — removing data |
| **200** | ✅ **2** thumbs up |
| **404** | ❌ **4**ault of user — wrong address |
| **500** | 💥 **5**erver is broken |
| **HTTPS** | The **S** = **S**ecure, **S**ealed, **S**afe |
| **HTTP/1.1** | **1** connection for the whole **1** page |
| **SYN/ACK** | **Syn**chronize then **Ack**nowledge |

---

## 📝 Quick Summary (30-Second Recap)

1. You type a URL → browser finds the server's IP via **DNS**
2. Browser opens a connection to the server using **TCP**
3. Browser sends an **HTTP GET request** asking for the page
4. Server replies with a **status code** (200 = found, 404 = not found) + the content
5. **HTTP/1.0** opens a new connection per file; **HTTP/1.1** reuses one connection (faster)
6. **HTTPS** encrypts everything so your data can't be read by snoopers

---

*Notes compiled from Codecademy — API Fundamentals Module*
