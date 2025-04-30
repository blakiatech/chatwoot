# 🛠️ Chatwoot Professional Deployment Template (Self-Hosted)

This repository contains a **production-grade Docker Compose template** for deploying [Chatwoot](https://www.chatwoot.com) — an open-source customer engagement platform — securely and reliably on your own infrastructure.

It supports:
- Rails + Sidekiq deployment
- Scalable PostgreSQL + Redis backend
- Secure SMTP configuration (Zoho-compatible)
- HTTPS via reverse proxy (e.g., Traefik)
- Ready for Dokploy or Railway usage

---

## 🚀 What's Included

### ✅ Production-Ready Services

- `chatwoot-rails`: Web app container serving the Chatwoot interface.
- `chatwoot-sidekiq`: Background job processor for emails, messages, etc.
- `chatwoot-postgres`: PostgreSQL database with pgvector support.
- `chatwoot-redis`: Redis instance for caching and queueing.

### ✅ Environment-Based Config

The deployment uses a centralized `.env` file for clean configuration of services and secrets.

---

## 📁 Folder Structure

```
├── docker-compose.yml         # Main deployment file
├── .env                       # Your production environment variables (NOT committed)
├── .env.production.example    # Example env file
├── README.md                  # This documentation
```

---

## 🧪 How to Use

### 1. Clone This Repository

```bash
git clone https://github.com/your-org/chatwoot-deploy-template.git
cd chatwoot-deploy-template
```

### 2. Create and Configure `.env`

```bash
cp .env.production.example .env
```

Edit `.env` with secure values. Use your Zoho credentials and preferred domain.

> Example variables included for SMTP, PostgreSQL, Redis, and more.

### 3. Start the Stack

```bash
docker compose up -d
```

---

## 🌐 Domain Support (HTTPS)

Make sure to expose Chatwoot behind a reverse proxy like **Traefik** or **NGINX**, and configure:

```env
FRONTEND_URL=https://chat.yourdomain.com
FORCE_SSL=true
```

---

## 🔐 SMTP Email Configuration

Supports Zoho or any other SMTP provider:

```env
MAILER_DELIVERY_METHOD=smtp
MAILER_SENDER_EMAIL=Your Name <you@domain.com>
SMTP_DOMAIN=domain.com
SMTP_ADDRESS=smtp.zoho.eu
SMTP_PORT=587
SMTP_USERNAME=you@domain.com
SMTP_PASSWORD=your_password
SMTP_AUTHENTICATION=login
SMTP_ENABLE_STARTTLS_AUTO=true
SMTP_OPENSSL_VERIFY_MODE=none
```

This will enable email confirmations, notifications, and agent invites.

---

## ⚙️ Service Details

### chatwoot-rails
- Web UI + API
- Runs `rails s` after `db:chatwoot_prepare`

### chatwoot-sidekiq
- Background job processor (email sending, message queues)

### PostgreSQL (pgvector)
- Preconfigured for AI features (e.g., embedding search)

### Redis
- Caching + Sidekiq backend

---

## 📦 Storage

All persistent data is stored in Docker volumes:

```yaml
volumes:
  chatwoot-storage:
  chatwoot-postgres-data:
  chatwoot-redis-data:
```

You can mount backups or integrate with S3 manually if needed.

---

## 🧪 Example `.env.production.example`

```env
FRONTEND_URL=https://chat.example.com
SECRET_KEY_BASE=REPLACE_WITH_SECURE_KEY
RAILS_ENV=production
NODE_ENV=production
INSTALLATION_ENV=docker

POSTGRES_HOST=chatwoot-postgres
POSTGRES_PORT=5432
POSTGRES_DATABASE=chatwoot
POSTGRES_USERNAME=postgres
POSTGRES_PASSWORD=REPLACE_DB_PASSWORD

REDIS_URL=redis://chatwoot-redis:6379
ENABLE_ACCOUNT_SIGNUP=false

MAILER_DELIVERY_METHOD=smtp
MAILER_SENDER_EMAIL=you@domain.com
SMTP_DOMAIN=domain.com
SMTP_ADDRESS=smtp.zoho.eu
SMTP_PORT=587
SMTP_USERNAME=you@domain.com
SMTP_PASSWORD=REPLACE_SMTP_PASS
SMTP_AUTHENTICATION=login
SMTP_ENABLE_STARTTLS_AUTO=true
SMTP_OPENSSL_VERIFY_MODE=none
```

---

## 🧼 Tips

- If using Dokploy or Railway, configure domain + reverse proxy outside the stack.
- You can add SSL termination and redirects via Traefik or Nginx Proxy Manager.
- Always rotate sensitive keys in production.

---

## 📎 License

MIT or custom license depending on your organization needs.

---

## ✍️ Credits

Template created and maintained by [Franblakia](https://github.com/fullfran) for [BlakIA](https://www.blakia.es).

Powered by [Chatwoot](https://www.chatwoot.com), PostgreSQL, Redis and Docker Compose.

