# PORTSWIGGER WEB SECURITY ACADEMY - PERSONAL LAB WRITE-UPS

This repository is my personal knowledge base for labs solved on the [PortSwigger Web Security Academy](https://portswigger.net/web-security).

I use it to:
- track progress topic by topic
- document exploit methodology step-by-step
- keep payloads and patterns I can quickly revisit


------

## Learning Goal

My goal is not only to "solve labs", but to understand **why** each exploit works and how to recognize the same vulnerability patterns in real assessments.

----------

## Current Progress


| Topic | Solved | Folder |
|---|---:|---|
| SQL Injection | 13 | [`SQL Injection/`](./SQL%20Injection/) |
| Cross-Site Scripting (XSS) | 7 | [`XSS/`](./XSS/) |
| Path Traversal | 6 | [`Path Traversal/`](./Path%20Traversal/) |
| Access Control | 5 | [`Access Control/`](./Access%20Control/) |
| Authentication | 4 | [`Authentication/`](./Authentication/) |
| Command Injection | 4 | [`Command Injection/`](./Command%20Injection/) |
| Cross-Site Request Forgery (CSRF) | 4 | [`CSRF/`](./CSRF/) |
| Cross-Origin Resource Sharing (CORS) | 3 | [`CORS/`](./CORS/) |
| Clickjacking | 1 | [`Clickjacking/`](./Clickjacking/) |
| **Total** | **47** | — |

---

## Repository Layout

Each folder represents a vulnerability category from the Academy.

Inside each folder, files follow this naming format:

```text
<index>. <official lab title>.md
```

Example:

```text
01. SQL injection vulnerability in WHERE clause allowing retrieval of hidden data.md
```

---

## Tooling I Use

- **Burp Suite** (primary testing workflow), **OWASP ZAP**, **Wireshark**
- Browser + DevTools
- Occasional helper scripting when useful for repetition

---

## How to Use This Repo

If you're learning along with these notes:

1. Pick a topic folder.
2. Open labs in numeric order (recommended for fundamentals).
3. Reproduce payloads manually first.
4. Compare your request/response flow with the write-up.
5. Keep your own notes for alternative payloads and bypasses.

---

## Notes

- These write-ups are for **educational use in legal lab environments**.
- If I make a mistake in a write-up, feel free to reach out :)
