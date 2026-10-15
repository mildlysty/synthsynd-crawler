# SynthSynd Crawler

> Open-source crawler for the crypto web. Real pages, real database, real-time SSE stream.

[![Rust](https://img.shields.io/badge/rust-1.83+-orange?logo=rust)](https://rust-lang.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Model](https://img.shields.io/badge/🤗_HuggingFace-the--broker--v1-blue)](https://huggingface.co/mildlysty/the-broker-v1)
[![Status](https://img.shields.io/badge/status-live-brightgreen)]()

## What it is

A production-ready Rust crawler that crawls crypto documentation across five cartels (Solana, Ethereum, Base, Bitcoin, Shadow). Every page is:

- Fetched from a real headless HTTP client
- Checked against `robots.txt`
- Extracted to clean text (scripts, navs, footers stripped)
- Hashed with BLAKE3 for deduplication
- Stored in SQLite
- Streamed live via Server-Sent Events to a browser

**Trained model is public:** [**the-broker-v1 on HuggingFace →**](https://huggingface.co/mildlysty/the-broker-v1)

**Live viewer:** [super-engine-x5v56j66jc66wr-8000.app.github.dev](https://super-engine-x5v56j66jc66wr-8000.app.github.dev)

## Architecture

```
┌─────────────────────────────────────────┐
│   Axum HTTP + SSE server (port 8000)    │
├─────────────────────────────────────────┤
│   Crawler workers (Tokio tasks)         │
├─────────────────────────────────────────┤
│   SQLite database (synthsynd.db)        │
├─────────────────────────────────────────┤
│   45 crypto domains · 5 cartels         │
└─────────────────────────────────────────┘
```

- **Language:** Rust
- **Web framework:** Axum
- **Async runtime:** Tokio
- **Database:** SQLite (via rusqlite)
- **HTML parsing:** scraper
- **Hashing:** blake3
- **Robots.txt:** custom parser, per-host cache
- **Streaming:** Server-Sent Events

## Quick start

```bash
# Clone
git clone https://github.com/mildlysty/synthsynd-crawler.git
cd synthsynd-crawler

# Run
cargo run --release
