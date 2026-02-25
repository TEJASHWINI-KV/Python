# 📡 API Fundamentals — Quick Reference Notes

> Personal study notes covering the full API module.  
> Includes concepts, analogies, memory tricks, and examples.

---

## 📌 TABLE OF CONTENTS
1. [What is an API?](#1-what-is-an-api)
2. [API Development Lifecycle](#2-api-development-lifecycle)
3. [Why APIs Get Deprecated](#3-why-apis-get-deprecated)
4. [API Design](#4-api-design)
5. [Characteristics of a Well-Designed API](#5-characteristics-of-a-well-designed-api)
6. [Collections, Resources & URLs](#6-collections-resources--urls)
7. [HTTP Methods (CRUD)](#7-http-methods-crud)
8. [Requests & Query Parameters](#8-requests--query-parameters)
9. [Responses & HTTP Status Codes](#9-responses--http-status-codes)
10. [API Documentation](#10-api-documentation)
11. [Master Glossary](#11-master-glossary)

---

## 1. What is an API?

**Definition:** A messenger that lets two different systems talk to each other.

> 🍽️ **Analogy:** A waiter in a restaurant.
> - You (client) = the one asking
> - Kitchen (server) = the one with the data
> - Waiter (API) = the one carrying requests and responses between them

**Example:**
- Weather app on your phone uses an API to fetch weather data from a remote server.
- You don't see the server — the API handles the communication.

---

## 2. API Development Lifecycle

**5 Stages (in order):**

```
Plan → Develop → Test → Deploy & Manage → Retire
```

| Stage | What Happens | Analogy |
|---|---|---|
| **1. Planning & Design** | Map out features, users, requirements, architecture | Architect drawing a blueprint |
| **2. Development** | Write the actual code; also write documentation | Building the house |
| **3. Testing** | Check for bugs, performance issues, edge cases | Test kitchen / food tasting |
| **4. Deploy & Manage** | Go live; monitor performance, usage, feedback | Grand opening + daily operations |
| **5. Retire (Deprecate)** | Shut down old version; migrate users to new version | Closing the restaurant |

> 🧠 **Memory Trick:** **"Please Do Test, Deploy, Retire"**  
> **P**lan → **D**evelop → **T**est → **D**eploy → **R**etire

---

## 3. Why APIs Get Deprecated

> **"It's working — why kill it?"**

Because **A and B don't stay the same forever.** The world changes, even if the API doesn't.

| Reason | Simple Explanation |
|---|---|
| 🦕 Technology gets outdated | Like a fax machine — better tech replaces it |
| 🔒 Security vulnerabilities | Old code develops exploitable holes |
| 💸 Cost | Maintaining old APIs costs money for no return |
| 🔄 Better version exists | v2 is faster/more powerful, so v1 must retire |
| 📦 Business changes | Company pivots, merges, or drops a product |
| 🧹 Technical debt | Old code becomes too messy to maintain safely |

> 🧠 **Key Insight:**
> ```
> Old API + Changing World = Eventually Broken, Insecure, or Irrelevant
> ```
> Deprecation = responsible engineering, NOT failure.

> 🏠 **Analogy:** A landline telephone works fine — but when B upgrades to a smartphone needing video calls and texts, the landline simply can't keep up.

---

## 4. API Design

**Definition:** The planning phase before writing any code — deciding what the API will do, who it serves, and how it will work.

> 🏛️ **Analogy:** An architect drawing a blueprint before a house is built.

**Benefits of Good API Design:**

| Benefit | Simple Meaning |
|---|---|
| **Effective Implementation** | Clear plan = no guessing for developers building it |
| **Incremental Development** | Easy to update specific parts without breaking everything |
| **Better Documentation** | Clean design = easier and faster to document |

> 🧠 **Memory Trick:** **"EIB"** — Effective, Incremental, Better docs

---

## 5. Characteristics of a Well-Designed API

> 🎮 **Analogy:** A TV remote — clear buttons, hard to break, does everything, has a manual, works every time.

| Characteristic | Meaning | Remote Analogy |
|---|---|---|
| **Easy to read & use** | Developers memorize it quickly | Obvious, labeled buttons |
| **Hard to misuse** | Guides correct usage; gives clear error messages | Buttons that can't be pressed wrong |
| **Complete & concise** | Returns exactly what's needed, nothing missing or extra | All needed buttons, no clutter |
| **Well documented** | Full guide covering all features and endpoints | Comes with a clear manual |
| **Reliable** | Always available; doesn't change without notice | Works every single time |

> 🧠 **Memory Trick:** **"ECCWR"** — Easy, Complete, Consistent, Well-documented, Reliable

---

## 6. Collections, Resources & URLs

### Use NOUNS, not VERBS

**URL = the address of a THING, not a description of the action.**

| ❌ Bad (Verb-based) | ✅ Good (Noun-based) |
|---|---|
| `/retrieveEveryPhoto` | `/photos` |
| `/getSinglePhoto/1` | `/photos/1` |
| `/deleteUserAccount/5` | `/users/5` |

**Why nouns?**
- Consistent across all endpoints
- Short and memorable
- The ACTION is handled by HTTP methods (not the URL)

> 🧠 **Memory Trick:** URL = the WHAT. HTTP Method = the HOW.

**Singular vs Plural:** No strict rule — just be **consistent** throughout your API.
- Either always `/photo` or always `/photos` — never mix both.

---

## 7. HTTP Methods (CRUD)

**CRUD = Create, Read, Update, Delete** — the 4 basic operations on any data.

| HTTP Method | CRUD Action | Description | Example |
|---|---|---|---|
| **GET** | Read | Retrieve data | `GET /photos/1` |
| **POST** | Create | Add new data | `POST /photos` |
| **PUT** | Update (full) | Replace entire resource | `PUT /photos/34` |
| **PATCH** | Update (partial) | Update only specific fields | `PATCH /photos/4` |
| **DELETE** | Delete | Remove a resource | `DELETE /photos/12` |

> 🧠 **Memory Trick:** **"Get Posted, Put a Patch, then Delete"**  
> GET → POST → PUT → PATCH → DELETE

**Combined URL + Method = Full meaning:**
```
GET    /photos      → Show me ALL photos
GET    /photos/1    → Show me photo #1
POST   /photos      → Upload a NEW photo
PATCH  /photos/4    → Update ONLY the hashtag on photo #4
DELETE /photos/12   → DELETE photo #12
```

---

## 8. Requests & Query Parameters

### What is a Query Parameter?
A filter or extra instruction added to a URL to narrow down results.

**Structure:**
```
/resource?key=value&key=value&key=value
```

| Part | Meaning |
|---|---|
| `?` | "Filters are starting here" |
| `key=value` | One filter (e.g., `location=boston`) |
| `&` | "And another filter..." |

**Real Example:**
> *"Get the top 10 photos from Boston with hashtag #winter"*
```
GET /photos?location=boston&hashtag=winter&limit=10
```

**Why use `limit`?**
- Prevents returning millions of results
- Protects server from overload
- Makes the app faster

> 💡 **"When in doubt, leave it out"**
> Don't try to build every feature upfront. Launch simple → get user feedback → improve. Over-engineering = wasted effort.

---

## 9. Responses & HTTP Status Codes

### The 3 Outcome Categories

| Code Range | Meaning | Delivery Analogy |
|---|---|---|
| **2xx ✅** | Success — it worked | "Package delivered" |
| **4xx ❌** | Client error — YOU did something wrong | "Wrong address — your fault" |
| **5xx 💥** | Server error — the API broke | "Our truck broke down — our fault" |

### Common Status Codes

| Code | Name | Meaning |
|---|---|---|
| `200` | OK | Request succeeded |
| `201` | Created | New resource successfully created |
| `400` | Bad Request | You sent something invalid |
| `401` | Unauthorized | You're not logged in |
| `403` | Forbidden | You don't have permission |
| `404` | Not Found | Resource doesn't exist |
| `500` | Internal Server Error | The API crashed |

> 🧠 **Memory Trick:**
> - **2** = ✅ **2**-thumbs up (success)
> - **4** = ❌ **4**-get out (your fault)
> - **5** = 💥 **5**-alarm fire (server's fault)

**Good error responses should:**
- ✅ Tell you WHAT went wrong
- ✅ Tell you WHY it went wrong
- ✅ Point to documentation if needed

```
❌ Bad:  "Error 400"
✅ Good: "Error 400: Missing required field 'location'. See /docs/photos"
```

---

## 10. API Documentation

**Definition:** The instruction manual for an API — a written guide explaining how to use it.

> 📺 **Analogy:** A new microwave with 20 buttons and no manual = useless to most people. Great docs = great microwave manual.

### What's Inside API Docs?

| Content | Meaning |
|---|---|
| **Tutorials** | Step-by-step walkthroughs |
| **Examples** | Real code showing actual usage |
| **Functions** | Specific actions the API performs |
| **Return Types** | What kind of data comes back (text, number, list) |
| **Arguments/Parameters** | Inputs you send to the API |

> ⚠️ Note: The **actual internal code** is NOT part of documentation. Docs explain *how to use* it, not *how it's built*.

### 5 Benefits of Good API Docs

| Benefit | Simple Meaning |
|---|---|
| **Improved Adoption** | Clear docs → more developers use your API |
| **Better Developer Experience (DX)** | Smooth, fast, frustration-free integration |
| **Increased Awareness** | Happy users recommend it to others (word-of-mouth) |
| **Saves Support Time & Cost** | Users self-serve → fewer support tickets |
| **Easier Maintenance** | Team always knows what each part does |

> 🧠 **Memory Trick:** **"ADASM"** — Adoption, DX, Awareness, Support savings, Maintenance

```
Great Docs → Developers understand quickly (DX)
           → More adoption
           → Happy users spread the word (Awareness)
           → Fewer support tickets (Saves Cost)
           → Easier to maintain and update
```

---

## 11. Master Glossary

| Term | Simple Meaning |
|---|---|
| **API** | Messenger between two systems |
| **API Lifecycle** | The full journey of an API: Plan → Build → Test → Deploy → Retire |
| **API Design** | Planning phase — blueprint before coding |
| **Endpoint** | A specific URL where an API can be accessed (e.g., `/photos`) |
| **Resource** | A "thing" the API manages (e.g., users, photos, orders) |
| **Collection** | A group of resources (e.g., all photos) |
| **Sub-resource** | A resource inside another (e.g., `/users/1/photos`) |
| **HTTP** | The communication protocol used on the internet |
| **HTTP Method** | The action (GET, POST, PUT, PATCH, DELETE) |
| **CRUD** | Create, Read, Update, Delete — 4 basic data operations |
| **Query Parameter** | A filter added to a URL after `?` |
| **Request** | What you send TO the API |
| **Response** | What the API sends BACK |
| **Status Code** | 3-digit number showing success or failure type |
| **REST / RESTful API** | API following standard HTTP method conventions |
| **Documentation** | The instruction manual for an API |
| **End-User** | The developer or person consuming the API |
| **Adoption** | How widely an API gets used |
| **Developer Experience (DX)** | How smooth it is for a dev to discover, learn, and use an API |
| **Onboarding** | Getting new users started with an API |
| **Integration** | Connecting your app to an external API |
| **Deprecation / Retiring** | Shutting down an old API version responsibly |
| **Technical Debt** | Accumulated mess from shortcuts taken during development |
| **Versioning** | Releasing updated iterations of an API (v1, v2, v3...) |
| **Evangelism** | Passionate users promoting your API to others |
| **Critical Mass** | When enough users exist that growth becomes self-sustaining |

---

> 📁 *Module: API Fundamentals | Source: SmartBear / Postman Learning*  
> 🗓️ *Last updated: February 2026*
