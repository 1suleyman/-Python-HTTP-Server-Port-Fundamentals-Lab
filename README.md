# 🌐 Python HTTP Server & Port Fundamentals Lab

In this lab, I learned how to **run a local web server, understand ports, debug connection issues, and distinguish between HTTP vs HTTPS at a protocol level**.

---

## 📋 Lab Overview

**Goal:**

* Run a Python HTTP server locally
* Serve files from a directory
* Understand ports, localhost, and URL structure
* Explore HTTP requests, responses, and errors
* Test protocol mismatches (HTTP vs HTTPS)
* Understand port conflicts and multiple services

**Learning Outcomes:**

* Use `python3 -m http.server` to run a web server
* Understand what `-m` and `http.server` do
* Correctly structure URLs (`host:port/path`)
* Identify HTTP status codes (200, 404)
* Understand port binding and conflicts
* Differentiate transport vs application protocols
* Recognise why HTTPS fails without TLS configuration

---

## 🛠 Step-by-Step Journey

### Step 1: Create Working Directory

**Commands:**

```bash
cd ..
cd ..
mkdir port-lab
cd port-lab
```

* Navigated directories and created a workspace for the lab

---

### Step 2: Create a Web Page

**Command:**

```bash
echo "Hello from port 8000" > index.html
```

* Creates `index.html` if it doesn’t exist
* Redirects output into the file

---

### Step 3: Start HTTP Server (Port 8000)

```bash
python3 -m http.server 8000
```

**What I learned:**

* `python3` → runs Python
* `-m` → runs a module as a script
* `http.server` → built-in module that creates a web server
* The server serves the **current directory**
* Port `8000` = where the server listens

---

### Step 4: Access the Server

**URL:**

```
http://localhost:8000
```

* `localhost` = this machine (`127.0.0.1` or `::1`)
* Successfully saw:

```
Hello from port 8000
```

---

### Step 5: Understand HTTP Logs

Example log:

```
GET / HTTP/1.1 200
GET /favicon.ico HTTP/1.1 404
```

**Breakdown:**

* `GET /` → browser requests homepage
* `HTTP/1.1` → protocol version
* `200` → success
* `404` → resource not found (e.g. favicon)

---

### Step 6: URL Structure Fix (Important Learning)

❌ My mistake:

```
localhost/about:8000
```

✅ Correct:

```
http://localhost:8000/about.html
```

**Key Insight:**

* `host:port` → identifies the server
* `/path` → identifies the file/resource

---

### Step 7: Default File Behaviour

* If `index.html` exists → server serves it
* If NOT → Python shows directory listing

**Example:**

```
port-lab/
about.html
test.txt
```

---

### Step 8: Security Insight (Directory Listing)

**Risk:**

* Exposes sensitive files like:

  * `.env`
  * `config.yaml`
  * backups

**Impact:**

* Attackers can discover:

  * credentials
  * API keys
  * internal structure

---

### Step 9: Stop Server

```bash
Ctrl + C
```

* Server stops
* Browser shows: “site can’t be reached”

---

### Step 10: Run Server on New Port (9000)

```bash
echo "Hello from port 9000" > index.html
python3 -m http.server 9000
```

**Test:**

```
http://localhost:9000
```

✅ Output:

<img width="563" height="211" alt="Screenshot 2026-04-29 at 17 34 17" src="https://github.com/user-attachments/assets/c0f6250b-f963-44ea-97d1-9891d4eda594" />

---

### Step 11: Port Understanding (Mental Model)

**Key Concept:**

* Port = “door” to a service
* Correct port = correct service

**Example:**

* Wrong port → connection fails
* Right port but wrong protocol → errors

---

### Step 12: Protocol Mismatch (HTTP vs HTTPS)

❌ Tried:

```
https://localhost:9000
```

**Error:**

```
ERR_SSL_PROTOCOL_ERROR
```

**Why:**

* Server speaks **HTTP**
* Browser expects **HTTPS (TLS encryption)**

---

### Step 13: HTTPS Requirement

To support HTTPS:

* Server must:

  * Have a certificate
  * Enable TLS encryption

❗ Key Insight:

> Certificate alone is NOT enough — server must actually speak HTTPS

---

### Step 14: Port Conflict Test

**Command (second terminal):**

```bash
python3 -m http.server 9000
```

**Error:**

```
Address already in use
```

**What I learned:**

* Only ONE process can bind to a port + IP combination
* Ports cannot be reused simultaneously

---

### Step 15: Multiple Servers on Different Ports

**Example:**

```bash
python3 -m http.server 8000
python3 -m http.server 9000
```

✅ Works because:

* Different ports = different entry points

---

### Step 16: Default Ports

| Protocol | Default Port |
| -------- | ------------ |
| HTTP     | 80           |
| HTTPS    | 443          |

**Examples:**

```
http://localhost        → port 80
https://localhost       → port 443
https://localhost:8443  → custom HTTPS port
```

---

### Step 17: Application vs Transport Layer Insight

**Key Learning:**

* Port open ≠ working application

**Example:**

* Connect browser to SSH port:

  * Connection works (TCP)
  * Response fails (wrong protocol)

---

## ✅ Key Commands Summary

| Task                    | Command                       |
| ----------------------- | ----------------------------- |
| Create directory        | `mkdir port-lab`              |
| Navigate directory      | `cd port-lab`                 |
| Create HTML file        | `echo "text" > index.html`    |
| Start server            | `python3 -m http.server 8000` |
| Stop server             | `Ctrl + C`                    |
| Start on different port | `python3 -m http.server 9000` |

---

## 💡 Notes / Tips

* `-m` = run Python module as script
* `http.server` serves current directory
* `localhost` = your own machine
* URL format = `http://host:port/path`
* Default file = `index.html`
* Directory listing = security risk
* Port must match service
* Protocol must match service (HTTP vs HTTPS)
* One port can only be used once per IP

---

## ✅ References

* Python http.server module
* Hypertext Transfer Protocol
* Transport Layer Security
