# Ports Notes

## Purpose of Ports

**Ports** are 16-bit unsigned integers (0 - 65535) that act as **logical endpoints** for network communications.

**Key Functions:**

- Distinguish between multiple applications/services running on the **same IP address**.
- Enable **multiplexing**: One network interface handles traffic for many processes.

Suppose there is a VM and it has multiple application running and VM has one IP address. To connect to any particular application we need ports.
Ports + IP = **Socket** (full endpoint identifier).

See [IP Address notes](ip-address.md) for IP basics.

## Ports in URLs & Connections

When connecting to websites/apps:

- `http://192.168.1.100` → Port **80** (default for HTTP)
- `https://example.com:443` → Port **443** (explicit)
- `http://localhost:8080` → Custom/development port **8080**
- `ftp://server:21` → FTP port **21**

**Default ports** omitted in URLs for convenience.

## Port Number Ranges

| Category              | Range         | Examples            | Assigned By              |
| --------------------- | ------------- | ------------------- | ------------------------ |
| **Well-known**        | 0 - 1023      | 80, 443, 22         | IANA (standard services) |
| **Registered**        | 1024 - 49151  | 8080, 3000          | App developers register  |
| **Dynamic/Ephemeral** | 49152 - 65535 | Random client ports | OS assigns temporarily   |

## Why Ports? One IP, Multiple Apps

IP identifies **host** (e.g., VM/server). Ports identify **service on host**.

**Problem solved**: Single VM IP runs web server, database, API simultaneously.

**Full Example** (`172.16.3.10` from [ip-address.md](ip-address.md)):

| Service         | Port | Protocol | Description        | Connection Example              |
| --------------- | ---- | -------- | ------------------ | ------------------------------- |
| HTTP Web        | 80   | TCP      | Website            | http://172.16.3.10:80           |
| HTTPS           | 443  | TCP      | Secure website     | https://172.16.3.10             |
| Development API | 8080 | TCP      | Backend/Custom app | http://172.16.3.10:8080         |
| SSH Login       | 22   | TCP      | Remote access      | ssh user@172.16.3.10:22         |
| MySQL DB        | 3306 | TCP      | Database           | mysql -h172.16.3.10 -P3306      |
| PostgreSQL      | 5432 | TCP      | Database           | psql host=172.16.3.10 port=5432 |
| Redis           | 6379 | TCP      | Cache/Key-value    | redis-cli -h 172.16.3.10        |
| Docker          | 2375 | TCP      | Docker daemon      | (Private usually)               |

**ASCII VM Diagram**:

```
Clients ──→ Firewall (NSG) ──→ VM IP: 172.16.3.10
                         ├── Port 80 ── Nginx/Apache
                         ├── Port 443 ── SSL Proxy
                         ├── Port 8080 ── Node.js/Express
                         ├── Port 3306 ── MySQL (internal)
                         └── Port 22 ─── OpenSSH
```

In Azure [subnet.md](subnet.md), use NSGs to open only required ports per subnet.

## TCP vs UDP

Ports work with two main protocols:

| Aspect         | TCP                             | UDP                               |
| -------------- | ------------------------------- | --------------------------------- |
| **Full Form**  | Transmission Control Protocol   | User Datagram Protocol            |
| **Connection** | Connection-oriented (handshake) | Connectionless (fire-and-forget)  |
| **Reliable**   | Yes (acks, retransmits, order)  | No (best-effort)                  |
| **Use Cases**  | Web (80/443), SSH, DB           | Video streaming, DNS (53), gaming |
| **Ports**      | Same ranges                     | Same ranges                       |

Example: HTTP uses TCP:80/443 for reliable page loads.

## Common Ports Quick Reference

| Port  | Service  | Protocol | Notes             |
| ----- | -------- | -------- | ----------------- |
| 20/21 | FTP      | TCP      | File transfer     |
| 22    | SSH      | TCP      | Secure shell      |
| 23    | Telnet   | TCP      | Insecure (avoid)  |
| 25    | SMTP     | TCP      | Email send        |
| 53    | DNS      | UDP/TCP  | Domain resolution |
| 80    | HTTP     | TCP      | Websites          |
| 110   | POP3     | TCP      | Email retrieve    |
| 143   | IMAP     | TCP      | Email (modern)    |
| 443   | HTTPS    | TCP      | Secure websites   |
| 993   | IMAPS    | TCP      | Secure IMAP       |
| 995   | POP3S    | TCP      | Secure POP3       |
| 8080  | HTTP-Alt | TCP      | Dev/Proxy         |
| 3306  | MySQL    | TCP      | Database          |
| 5432  | Postgres | TCP      | Database          |
| 6379  | Redis    | TCP      | Cache             |

