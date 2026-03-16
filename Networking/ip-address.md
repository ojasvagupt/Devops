# IP Address Notes

## Purpose

IP address is used to generate or provide a **unique address** to a particular **device** that is connected to a **network**.

**Key Functions:**

- **Host identification**: Distinguishes one device from another on a network.
- **Location addressing**: Indicates the device's location in the network.

## WiFi Example

- There is a **WiFi** (router) and **4 devices** are connected to it.
- Each device will have a **unique IP address** (assigned via DHCP in local network).

Example private IPs (e.g., subnet `172.16.3.0/24`):
| Device | IP Address |
|--------|--------------|
| Device 1 | 172.16.3.4 |
| Device 2 | 172.16.3.5 |
| Device 3 | 172.16.3.6 |
| Device 4 | 172.16.3.7 |

## Representation

- **IPv4**: Dotted decimal, e.g., `172.16.3.4`
- **IPv6**: Hexadecimal with colons, e.g., `2001:db8::1`

**Diagram** (IPv4 octet structure):

```
 8 bits  |  8 bits |  8 bits |  8 bits
---------|---------|---------|---------
Octet 1 | Octet 2 | Octet 3 | Octet 4
```

## Range

- IPv4: `(0-255).(0-255).(0-255).(0-255)` → Total ~4.3 billion addresses.

## Bits and Bytes (Corrections & Details)

- **IPv4**: 32 bits total = **4 bytes** (each octet = 8 bits = 1 byte).
  - _Correction: Not "8 bytes" per octet; octet is 8 bits (1 byte)._
- **IPv6**: 128 bits = 16 bytes.

## Public vs Private IPs

- **Public IP**: Unique globally, routable on the internet. Assigned by ISP (e.g., your router's WAN IP).
- **Private IP**: Used within local networks (not routable on internet). Defined ranges:

  | Range                         | Class | Typical Use         |
  | ----------------------------- | ----- | ------------------- |
  | 10.0.0.0 - 10.255.255.255     | A     | Large private       |
  | 172.16.0.0 - 172.31.255.255   | B     | Medium private      |
  | 192.168.0.0 - 192.168.255.255 | C     | Home/small networks |
