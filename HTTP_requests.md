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
12. [What is REST?](#12-what-is-rest)
13. [REST — Client & Server Separation](#13-rest--client--server-separation)
14. [REST — Statelessness](#14-rest--statelessness)
15. [REST Requests — Structure](#15-rest-requests--structure)
16. [MIME Types](#16-mime-types)
17. [REST Paths & URL Design](#17-rest-paths--url-design)
18. [REST Response Codes](#18-rest-response-codes)
19. [REST in Action — Full Examples](#19-rest-in-action--full-examples)
20. [REST Memory Tricks & Quick Reference](#20-rest-memory-tricks--quick-reference)

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

## 12. What is REST?

**REST = REpresentational State Transfer**

- An **architectural style** (a set of design principles) for building web services.
- Makes it easy for computer systems to **communicate with each other** over the web.
- Systems that follow REST rules are called **RESTful systems**.

```
Client  ──── HTTP Request ────→  RESTful Server
Client  ←─── HTTP Response ───  RESTful Server
```

### Two Core Pillars of REST:
1. **Separation of Client and Server**
2. **Statelessness**

> 🧠 **Memory Trick:** REST = **R**ules for **E**asy **S**ystem **T**alking

> 💼 **Interview Tip:** REST is one of the most commonly asked definitions in tech interviews. Know it cold!

---

## 13. REST — Client & Server Separation

The client and server are **completely independent** of each other.

```
Client (Frontend)          Server (Backend)
──────────────────         ─────────────────
React App        }         Node.js API      }  Can be changed
Mobile App       }  ────→  Python API       }  independently
Desktop App      }         Java API         }  without breaking
                                               each other
```

### Why this matters:
- You can **rebuild the entire frontend** without touching the backend.
- You can **swap out the database** without affecting the client.
- Multiple different clients (web, mobile, desktop) can all use the **same REST API**.
- Each side can **evolve independently**.

> 🏢 **Analogy:** A restaurant kitchen (server) and its waitstaff (client). The kitchen can change its recipes and equipment — as long as it still delivers the dishes the menu (API contract) promises, the waitstaff doesn't care.

---

## 14. REST — Statelessness

**Stateless** = The server remembers **nothing** about the client between requests.

```
Request 1: GET /customers/123    ← Server handles it, then forgets
Request 2: GET /orders/456       ← Server treats this as brand new
Request 3: DELETE /customers/123 ← Server still has no memory of Request 1
```

Every request must contain **all the information** needed to process it — the server does not store session/context between calls.

### Why this is good:
| Benefit | Explanation |
|---------|-------------|
| **Reliability** | No session data to corrupt or lose |
| **Scalability** | Any server in a cluster can handle any request |
| **Performance** | Servers don't waste memory storing client state |

> 🛒 **Analogy:** Like ordering at a fast food counter where the cashier has no memory of your previous visits. Every time you walk up, you give your full order from scratch. They don't assume anything.

> 🧠 **Memory Trick:** **State**less = the server holds **no state** about you between requests.

---

## 15. REST Requests — Structure

A REST request has up to **4 parts**:

```
┌─────────────────────────────────────────────┐
│  1. HTTP Verb    →  What action to perform   │
│  2. Header       →  Metadata about request   │
│  3. Path         →  Where the resource lives │
│  4. Body         →  Data to send (optional)  │
└─────────────────────────────────────────────┘
```

### The 4 HTTP Verbs in REST:

| Verb | Action | Returns on Success |
|------|--------|-------------------|
| `GET` | Retrieve a resource or list | `200 OK` |
| `POST` | Create a new resource | `201 CREATED` |
| `PUT` | Update an existing resource | `200 OK` |
| `DELETE` | Remove a resource | `204 NO CONTENT` |

> 🧠 **CRUD Memory Trick:**
> ```
> C → Create  → POST
> R → Read    → GET
> U → Update  → PUT
> D → Delete  → DELETE
> ```

---

## 16. MIME Types

**MIME = Multipurpose Internet Mail Extensions**

Used in request/response **headers** to tell both sides what **type of data** is being sent or expected.

### Format:
```
type/subtype
```

### Common MIME Types:

| Category | MIME Type | Used For |
|----------|-----------|---------|
| **Text** | `text/html` | HTML pages |
| **Text** | `text/css` | CSS stylesheets |
| **Text** | `text/plain` | Plain text |
| **Image** | `image/png` | PNG images |
| **Image** | `image/jpeg` | JPEG images |
| **Image** | `image/gif` | GIF images |
| **Audio** | `audio/wav` | WAV audio |
| **Audio** | `audio/mpeg` | MP3 audio |
| **Video** | `video/mp4` | MP4 video |
| **App** | `application/json` | JSON data (most common in REST) |
| **App** | `application/pdf` | PDF files |
| **App** | `application/xml` | XML data |

### How they're used in a request:
```http
GET /articles/23
Accept: text/html, application/xhtml
```
- `Accept` header = *"I can receive these content types"* (client → server)
- `Content-Type` header = *"Here's what I'm sending you"* (server → client)

> 🧠 **Memory Trick:** MIME = **M**essage **I**dentity for **M**edia **E**xchange — it identifies what kind of media is in the message.

---

## 17. REST Paths & URL Design

Paths tell the server **which resource** you want to act on.

### Rules for good REST path design:

```
✅ Use plural nouns for resources
✅ Use hierarchy to show relationships
✅ Use IDs to target specific items
❌ Don't use verbs in paths (the HTTP method IS the verb)
```

### Examples:

| Path | Meaning |
|------|---------|
| `GET /customers` | Get ALL customers |
| `GET /customers/223` | Get customer with id 223 |
| `POST /customers` | Create a new customer |
| `PUT /customers/223` | Update customer 223 |
| `DELETE /customers/223` | Delete customer 223 |
| `GET /customers/223/orders` | Get ALL orders for customer 223 |
| `GET /customers/223/orders/12` | Get order 12 of customer 223 |

> 🗂️ **Analogy:** Think of paths like a folder structure:
> ```
> /customers          ← the customers folder
> /customers/223      ← a specific customer's file
> /customers/223/orders/12  ← a specific order inside that customer
> ```

> 🧠 **Memory Trick:** Path = **address of the resource**. Make it so readable that anyone can guess what it does.

---

## 18. REST Response Codes

The server always replies with a **status code** to tell the client what happened.

### Full REST Status Code Reference:

| Code | Name | Meaning | Triggered By |
|------|------|---------|-------------|
| `200` | OK | ✅ Success | GET, PUT |
| `201` | CREATED | ✅ New resource made | POST |
| `204` | NO CONTENT | ✅ Success, nothing to return | DELETE |
| `400` | BAD REQUEST | ❌ Malformed/invalid request | Client sent bad data |
| `403` | FORBIDDEN | 🚫 No permission | Not authorized |
| `404` | NOT FOUND | ❌ Resource doesn't exist | Wrong path/id |
| `500` | INTERNAL SERVER ERROR | 💥 Server crashed | Server-side bug |

### Expected response per verb:
```
GET    → 200 (OK)
POST   → 201 (CREATED)
PUT    → 200 (OK)
DELETE → 204 (NO CONTENT)
```

> 🧠 **Memory Trick:**
> - **201** = **1** new thing **created** (POST always makes something new)
> - **204** = **No** content because you just **deleted** it — nothing left to return
> - **403** = You're **forbidden** — you exist but have no permission
> - **404** = It **doesn't exist** at that path

---

## 19. REST in Action — Full Examples

Using a clothing store API at `fashionboutique.com`:

### GET — Retrieve All Customers
```http
GET http://fashionboutique.com/customers
Accept: application/json
```
```http
200 (OK)
Content-Type: application/json
[{ "id": 1, "name": "Alice" }, { "id": 2, "name": "Bob" }]
```

---

### POST — Create a New Customer
```http
POST http://fashionboutique.com/customers
Body:
{
  "customer": {
    "name": "Scylla Buss",
    "email": "scylla@example.com"
  }
}
```
```http
201 (CREATED)
Content-Type: application/json
{ "id": 124, "name": "Scylla Buss", "email": "scylla@example.com" }
```

---

### GET — Retrieve a Single Customer
```http
GET http://fashionboutique.com/customers/123
Accept: application/json
```
```http
200 (OK)
Content-Type: application/json
{ "id": 123, "name": "Scylla Buss", "email": "scylla@example.com" }
```

---

### PUT — Update a Customer
```http
PUT http://fashionboutique.com/customers/123
Body:
{
  "customer": {
    "email": "newemail@example.com"
  }
}
```
```http
200 (OK)
```

---

### DELETE — Remove a Customer
```http
DELETE http://fashionboutique.com/customers/123
```
```http
204 (NO CONTENT)
```

---

## 20. REST Memory Tricks & Quick Reference

| Concept | Memory Trick |
|---------|-------------|
| **REST** | **R**ules for **E**asy **S**ystem **T**alking |
| **Stateless** | Server has **no memory** between requests — every call is fresh |
| **Client/Server separation** | Kitchen & waitstaff — independent, same menu contract |
| **GET** | *"Give me"* — retrieve |
| **POST** | *"Here's something new"* — create |
| **PUT** | *"Replace this"* — update |
| **DELETE** | *"Throw it away"* — remove |
| **200** | ✅ **2** thumbs up |
| **201** | ✅ **1** new thing created |
| **204** | ✅ Done, **nothing** left to return |
| **400** | ❌ **Bad** request — your fault |
| **403** | 🚫 **For**bidden — no permission |
| **404** | ❌ **Not** there |
| **500** | 💥 **S**erver broke |
| **MIME Type** | `type/subtype` format — identifies media type |
| **Accept header** | *"I can receive this type"* |
| **Content-Type header** | *"I am sending this type"* |
| **Path design** | Use plural nouns + IDs, no verbs |

### REST vs Non-REST at a Glance:
```
❌ Non-REST path:   /getCustomer?id=123
                    /deleteOrder?id=456
                    /createUser

✅ RESTful path:    GET    /customers/123
                    DELETE /orders/456
                    POST   /users
```

---

## 📝 Quick Summary (30-Second Recap)

### HTTP:
1. You type a URL → browser finds the server's IP via **DNS**
2. Browser opens a connection to the server using **TCP**
3. Browser sends an **HTTP GET request** asking for the page
4. Server replies with a **status code** (200 = found, 404 = not found) + the content
5. **HTTP/1.0** opens a new connection per file; **HTTP/1.1** reuses one connection (faster)
6. **HTTPS** encrypts everything so your data can't be read by snoopers

### REST:
1. REST is an **architectural style** — a set of rules for how APIs should be designed
2. **Stateless** — server remembers nothing between requests
3. **Client/Server separation** — both sides are independent
4. Requests use **HTTP verbs** (GET/POST/PUT/DELETE) to define the action
5. **Paths** use plural nouns and IDs to identify resources
6. **MIME types** in headers tell both sides what data format to expect
7. **Status codes** tell the client whether the operation succeeded or failed

---

*Notes compiled from Codecademy — API Fundamentals Module*
