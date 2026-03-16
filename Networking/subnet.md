# Subnetting and CIDR in Azure Virtual Networks

## Introduction

You've created an Azure Virtual Network (VNet). Next step: create subnets within it.

A **subnet** is a logical division of the VNet's IP address space. Subnets allow you to segment the network for security, organization, and performance.

Example VNet address space: **172.16.0.0/16** (172.16.0.0 to 172.16.255.255, ~65,536 IPs).

## Why Use Subnetting? (Security Isolation)

Imagine an office with **10,000 people** on WiFi. If one user visits a malicious website, the hacker could potentially access **all devices** on the network.

**Subnetting solves this**:

- Divides the big network into smaller **subnets**.
- Sensitive data (e.g., databases) in a separate subnet.
- Even if one subnet is compromised (e.g., user subnet), the sensitive subnet remains isolated.

Subnets are called 'sub' because they are **ranges within the larger VNet** (secured privacy isolation via Network Security Groups - NSGs).

See [IP Address notes](../ip-address.md) for IP basics.

## Types of Subnets

2 main types in Azure:

| Type        | Internet Access | Use Case                    | Configuration                     |
| ----------- | --------------- | --------------------------- | --------------------------------- |
| **Public**  | Yes             | Web servers, load balancers | Route table to Internet Gateway   |
| **Private** | No              | Databases, internal apps    | NSG blocks outbound, optional NAT |

- **NSGs** enforce firewall rules per subnet for extra security.

## CIDR Notation: How to Assign IP Addresses to Subnets

**CIDR (Classless Inter-Domain Routing)** defines a subnet's IP range and size.

Format: **network_address/prefix_length**

- **Network address**: First IP (e.g., 172.16.3.0).
- **/prefix_length** (e.g., /24): Number of fixed bits in the subnet mask (out of 32 for IPv4).

### How /24 Gives 256 IPs (and Usable IPs Calculation)?

- IPv4 = **32 bits** total.
- /24 = **24 network bits** → **8 host bits** (32 - 24 = 8).
- **Total addresses** = 2^8 = **256**.
- **Usable IPs for devices** = 256 - 2 = **254**.

**Why -2?**

```
IP:       172.16.3.10  → 10101100.00010000.00000011.00001010
Mask:     255.255.255.0 → 11111111.11111111.11111111.00000000 (/24)
```

- **Network ID**: 172.16.3.0 (first IP, identifies subnet - not for devices).
- **Usable Range**: 172.16.3.1 - 172.16.3.254 (assign to VMs/instances).
- **Broadcast**: 172.16.3.255 (last IP, for subnet broadcasts - not for devices).

**General Formula**:

- Host bits = 32 - prefix_length
- Total IPs = 2^host_bits
- Usable IPs = 2^host_bits - 2

**Azure Note**: Reserves 5 more IPs (e.g., /28 min size), but base formula applies.

### CIDR Prefix vs Number of IPs Table

| Prefix | Host Bits | Total IPs | Usable IPs | Example in 172.16.0.0/16 |
| ------ | --------- | --------- | ---------- | ------------------------ |
| /28    | 4         | 16        | 14         | 172.16.1.0/28            |
| /24    | 8         | 256       | 254        | 172.16.3.0/24            |
| /23    | 9         | 512       | 510        | 172.16.4.0/23            |
| /22    | 10        | 1,024     | 1,022      | 172.16.8.0/22            |
| /16    | 16        | 65,536    | 65,534     | 172.16.0.0/16 (VNet)     |

## Examples: Creating Subnets in VNet 172.16.0.0/16

Suppose subnets for office:

- **Public Web Subnet**: 254 IPs → 172.16.1.0/24
- **Private DB Subnet**: 254 IPs → 172.16.2.0/24
- **User WiFi Subnet** (10k users ~ /21 for 2048 IPs): 172.16.10.0/21

| Subnet Name | CIDR           | Usable IPs | Purpose                 |
| ----------- | -------------- | ---------- | ----------------------- |
| public-web  | 172.16.1.0/24  | 254        | Internet-facing apps    |
| private-db  | 172.16.2.0/24  | 254        | Sensitive databases     |
| wifi-users  | 172.16.10.0/21 | 2042       | Office WiFi (10k users) |

**Non-overlapping**: Must fit within VNet, no overlap.

**ASCII Diagram**:

```
VNet 172.16.0.0/16
├── Public: 172.16.1.0/24  [Internet ↑]
├── Private: 172.16.2.0/24 [Isolated]
└── Users: 172.16.10.0/21  [WiFi]
```

Your examples:

- 172.16.3.0/24 → 256 IPs (perfect for small subnet).
- Ranges like 172.16.3.0 to 172.16.3.255.

## Creating Subnet in Azure (Portal/CLI)

### Azure Portal:

1. VNet → Subnets → +Subnet.
2. Name: `private-db`, Address range: `172.16.2.0/24`, Security: Add NSG.

### Azure CLI:

```
az network vnet subnet create \
  --resource-group myRG \
  --vnet-name myVNet \
  --name private-db \
  --address-prefixes 172.16.2.0/24 \
  --network-security-group myNSG
```

## Best Practices

- Start with larger subnets, split later (can't merge).
- Public: Minimal, use NSGs.
- Private: Default for sensitive.
- Delegate subnets (e.g., for containers).
- Monitor usage to avoid exhaustion.

This setup ensures **one hacked device doesn't compromise everything**!
