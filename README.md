# 🌐 Cloudflare Integration Workflow – CicilyHomes Estates

[![Domain](https://img.shields.io/badge/Domain-cicilyhomes.com-blue?style=for-the-badge&logo=internet-explorer)](https://cicilyhomes.com)
[![DNS](https://img.shields.io/badge/DNS-Cloudflare-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)](https://cloudflare.com)
[![Hosting](https://img.shields.io/badge/Hosting-Hostinger-673DE6?style=for-the-badge&logo=hostinger&logoColor=white)](https://hostinger.com)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)]()

Official infrastructure documentation for configuring Cloudflare DNS, CDN, and security proxies for **`cicilyhomes.com`**, hosted on Hostinger.

---

## 📋 Metadata

| Property | Details |
| :--- | :--- |
| **Domain** | `cicilyhomes.com` |
| **Project** | CicilyHomes Estates |
| **Author / Lead** | **Chukwuemeka Nze** (Founder & Frontend Engineer) |
| **Date** | November 2026 |
| **Purpose** | Performance optimization, DDoS protection, and SSL management |

---

## 💡 Overview

This repository contains the architecture, step-by-step workflow, and operational checklist for delegating the DNS authority of `cicilyhomes.com` from Hostinger to Cloudflare while maintaining origin web hosting on Hostinger servers.

### **Core Objectives**
- 🚀 **Speed & Cache:** Edge caching to accelerate page loading speed globally.
- 🔒 **Security:** Free SSL/TLS termination, automatic HTTPS rewrites, and DDoS mitigation.
- 📊 **Observability:** Traffic analytics and threat intelligence via Cloudflare Dashboard.
- ⚡ **Resource Optimization:** Reduce origin server load and bandwidth usage on Hostinger.

---

## 🏗️ System Architecture

```text
                                  +-----------------------+
                                  |     End User / Client |
                                  +-----------+-----------+
                                              |
                                              v
                                  +-----------+-----------+
                                  |   Cloudflare Edge     |
                                  | (DNS, WAF, SSL, CDN)  |
                                  +-----------+-----------+
                                              |
                                              v (Proxied Traffic)
                                  +-----------+-----------+
                                  |   Hostinger Origin    |
                                  |    (Web Server)       |
                                  +-----------+-----------+
                                              |
                                              v
                                  +-----------+-----------+
                                  |   Paystack Gateway    |
                                  |  (Upcoming Integration) |
                                  +-----------------------+
