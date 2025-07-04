# CacheCraft: React Component CDN with Module Federation, Redis Caching, and Edge Delivery

CacheCraft is a production-grade, modular CDN platform designed to serve shared React components across multiple front-end applications using Webpack Module Federation, Redis-based usage tracking, Cloudflare Workers for edge delivery, Docker for containerization, and CI/CD pipelines for automation.

---

## 🧩 Key Features

- 🔌 On-demand, runtime loading of React components via Webpack Module Federation
- 💾 Intelligent caching using Redis (with LRU-based eviction policy)
- 🌍 Global edge delivery using Cloudflare Workers (low-latency performance)
- 📦 Decoupled packaging with Docker for scalability and portability
- ⚙️ CI/CD automation using GitHub Actions for consistent build, cache push, and deployment
- 🔁 Component versioning support for controlled rollouts and rollback

---

## 🚦 Full Architecture Flow

1. Component Provider App builds and exposes React components via Module Federation
2. Bundled outputs (remoteEntry.js and exposed modules) are uploaded to an object store (e.g. Cloudflare R2/S3)
3. Cloudflare Worker intercepts requests from host apps and:
   - Checks Redis for cached module payloads
   - If a cache miss occurs, fetches the payload from origin
   - Stores in Redis and returns the JavaScript
4. Host Apps dynamically load these components via Module Federation remotes
5. CI/CD automates steps 1–3 during each deployment or component update

---

## 🧱 Tech Stack Overview

| Layer                 | Tool/Technology              | Purpose                                     |
|----------------------|------------------------------|---------------------------------------------|
| Component Federation | Webpack 5 + Module Federation| Runtime sharing of modules between apps     |
| Component Rendering  | React                        | Core framework for reusable UI components   |
| Caching              | Redis + ZINCRBY              | LRU caching and access frequency tracking   |
| Edge Delivery        | Cloudflare Workers           | JavaScript delivery from edge servers       |
| CDN Storage          | AWS S3 / Cloudflare R2       | Stores built modules and manifests          |
| Packaging            | Docker                       | Containerized builds for portability        |
| Automation           | GitHub Actions               | CI/CD workflow for publishing and caching   |

---

## 📁 Project Structure

```bash
cachecraft/
├── provider/                     # Component library
│   ├── src/components/           # UI components: Button, Navbar, etc.
│   ├── webpack.config.js         # Module Federation exposer
│   ├── Dockerfile
│   └── package.json
│
├── worker/                       # Edge delivery worker
│   ├── index.js                  # Cloudflare Worker script
│   ├── redis.js                  # Redis cache wrapper
│   ├── wrangler.toml             # Cloudflare Worker config
│   ├── Dockerfile
│   └── package.json
│
├── host/                         # Example consumer app
│   ├── src/App.jsx
│   ├── webpack.config.js         # Module Federation consumer
│   └── package.json
│
├── .github/workflows/
│   └── ci.yml                    # CI/CD: build → upload → deploy
│
└── README.md
```
