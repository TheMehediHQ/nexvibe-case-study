# Nexvibe — Architecture & Engineering Case Study

[![Next.js 16](https://img.shields.io/badge/Next.js-16-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![React 19](https://img.shields.io/badge/React-19-blue?style=for-the-badge&logo=react)](https://react.dev/)
[![Tailwind CSS 4](https://img.shields.io/badge/Tailwind_CSS-4-38B2AC?style=for-the-badge&logo=tailwind-css)](https://tailwindcss.com/)
[![Upstash Redis](https://img.shields.io/badge/Upstash-Redis-00E599?style=for-the-badge&logo=redis)](https://upstash.com/)
[![Cloudflare Turnstile](https://img.shields.io/badge/Cloudflare-Turnstile-F38020?style=for-the-badge&logo=cloudflare)](https://www.cloudflare.com/products/turnstile/)

> **Confidentiality / NDA Disclaimer:**  
> The production source code of Nexvibe contains proprietary client business logic and intellectual property protected under Non-Disclosure Agreements (NDAs). To demonstrate engineering capabilities, system design, and security architecture without violating compliance standards, this repository serves as an **in-depth technical case study**.

---

## 📖 Quick Navigation

- [Executive Summary](#-executive-summary)
- [The Problem & Objectives](#-the-problem--objectives)
- [System Architecture](#-system-architecture)
- [Technology Stack](#-technology-stack)
- [Security & Defense-in-Depth](#-security--defense-in-depth)
- [Transactional Pipeline](#-transactional-pipeline)
- [Motion & UI/UX Engineering](#-motion--uiux-engineering)
- [Page Breakdown](#-page-breakdown)
- [Performance & Core Web Vitals](#-performance--core-web-vitals)
- [Detailed Case Study Document](./CASE_STUDY.md)

---

## 🚀 Executive Summary

**Nexvibe** is an AI-driven data solutions agency providing enterprise predictive analytics, data pipelines, and machine learning infrastructure.

This project entailed engineering an ultra-responsive, highly secure, visually captivating digital presence designed to:
1. Showcase complex artificial intelligence and big data service offerings.
2. Provide seamless, interactive case study exploration for enterprise clients.
3. Facilitate high-volume inbound business inquiries and job applications with enterprise-grade spam immunity and zero bot abuse.

---

## ⚡ The Problem & Objectives

- **High-End Motion without Performance Penalties:** Enterprise AI agencies require dynamic visual feedback (3D feels, interactive cursors, timeline animations). However, traditional script-heavy animations degrade Google Core Web Vitals (LCP, INP, and CLS).
- **Spam Floods on Public Ingestion Endpoints:** Inbound contact and careers endpoints are primary targets for spam bots and credential-stuffing crawlers. Traditional image/text CAPTCHAs severely harm enterprise conversion rates.
- **Reliable Inbound Delivery:** Misconfigured or rate-limited email forms can lead to lost high-ticket client leads and unorganized job applications.

---

## 🏗️ System Architecture

```
                          [ Client Browser ]
                                  |
                           (HTTPS / Next.js 16)
                                  |
                                  v
                    +-----------------------------+
                    |    Next.js 16 App Router    |
                    | (React Server Components)   |
                    +--------------+--------------+
                                   |
         +-------------------------+-------------------------+
         |                                                   |
         v                                                   v
[ Edge Security Pipeline ]                        [ Interactive View Layer ]
 - Cloudflare Turnstile                            - Tailwind CSS 4 Theming
 - Upstash Redis Rate Limiter                      - GSAP Timelines
 - Zod Runtime Schema Validation                   - Framer Motion Transitions
         |                                         - AOS Scroll Triggers
         v
[ Transactional Engine ]
 - Resend API
 - React Email Templates
 - Dual Notification (Team alert + Auto-reply)
```

---

## 🛠️ Technology Stack

| Category | Tool / Library | Reason for Selection |
|:---|:---|:---|
| **Framework** | **Next.js 16 (App Router)** | Cutting-edge RSC (React Server Components), TurboPack builds, and edge routing. |
| **UI Library** | **React 19** | Latest hydration, actions, and compiler performance improvements. |
| **Styling** | **Tailwind CSS 4** | Instant JIT compilation, native CSS variables for dynamic theming, zero config bloat. |
| **Animation** | **GSAP + Framer Motion + AOS** | GSAP for complex timeline control; Framer Motion for state-based components; AOS for performant scroll-reveals. |
| **Rate Limiting** | **Upstash Redis (`@upstash/ratelimit`)** | Serverless, ultra-low latency sliding window rate limiter at the edge. |
| **Bot Mitigation** | **Cloudflare Turnstile** | Privacy-preserving, frictionless CAPTCHA alternative providing superior conversion rates. |
| **Validation** | **Zod** | End-to-end schema integrity and type safety across client and server. |
| **Transactional Mail**| **Resend + React Email** | Component-driven, responsive HTML email generation with high inbox deliverability. |
| **Runtime / Package Mgr** | **Bun** | Ultra-fast package resolution, build, and script execution. |

---

## 🛡️ Security & Defense-in-Depth

The ingestion endpoints (`/api/contact` and `/api/careers`) follow a multi-layered defense architecture:

```
Incoming Request
      │
      ▼
[ 1. Rate Limiting Check ]  ──(Over limit)──> 429 Too Many Requests (Upstash Redis)
      │ (Pass)
      ▼
[ 2. Bot Challenge Verify ] ──(Invalid)─────> 403 Forbidden (Cloudflare Turnstile)
      │ (Pass)
      ▼
[ 3. Schema Validation ]    ──(Malformed)───> 400 Bad Request (Zod)
      │ (Pass)
      ▼
[ 4. Email Pipeline ]       ───────────────> Resend Transactional Dispatch
```

1. **Sliding-Window Rate Limiting:** Enforces strict burst and sustained limits per IP address via Upstash Redis without blocking legitimate corporate networks.
2. **Turnstile Bot Verification:** Validates encrypted challenge tokens with Cloudflare's API, rejecting headless automated scrapers and puppeteer instances before any payload processing.
3. **Strict Zod Sanitization:** Filters malicious input, prevents injection attacks, and enforces strict boundary rules on textual fields and resume metadata.

---

## 📧 Transactional Pipeline

Emails are rendered using `@react-email/components`:
- **For Internal Stakeholders:** Clean, structured notification emails containing candidate/lead data with direct reply headers.
- **For Leads / Applicants:** Instant, branded confirmation emails reassuring clients that their inquiry is in review by the Nexvibe solutions team.

---

## 🌐 Page Breakdown

| Route | Purpose & Implementation |
|:---|:---|
| `/` | **Home** — Interactive Hero with GSAP, dynamic Service overview, Portfolio highlights, Client Testimonials, and expandable FAQ accordions. |
| `/about` | **About** — Agency mission, executive leadership, technological philosophy, and core data values. |
| `/services` | **Services** — Detailed breakdown of offerings: Big Data Engineering, Predictive AI, and Cloud Infrastructure. |
| `/portfolio` | **Portfolio** — Interactive case study showcase highlighting measurable client business metrics. |
| `/careers` | **Careers** — Live job postings and an integrated application form with file upload support. |
| `/contact` | **Contact** — Lead generation form backed by rate-limiting, Turnstile protection, and real-time status UI. |
| `/privacy-policy` & `/terms-of-service` | Compliance documentation tailored for enterprise engagement. |

---

## 📈 Performance & Results

- **Lighthouse Performance Score:** 98+
- **Core Web Vitals:**
  - **LCP (Largest Contentful Paint):** < 1.2s
  - **CLS (Cumulative Layout Shift):** 0.00
  - **INP (Interaction to Next Paint):** < 50ms
- **Bot Mitigation Efficiency:** 100% automated spam blocked without impacting lead conversion.

---

## 💬 Contact & Technical Discussions

For questions regarding this architecture, technical design consultations, or professional inquiries, please reach out via GitHub or your preferred professional channel.

---
*Authored as an architectural demonstration for Nexvibe's enterprise platform.*
