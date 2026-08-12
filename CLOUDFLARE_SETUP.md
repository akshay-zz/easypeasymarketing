# Cloudflare Deployment Setup Guide

## Overview
This project is configured to deploy to Cloudflare Workers/Pages. The setup includes:

- **wrangler.toml** - Main Cloudflare configuration file
- Updated **next.config.js** - Cloudflare compatibility settings
- New npm scripts for deployment

## Prerequisites

1. **Cloudflare Account** - Sign up at [cloudflare.com](https://www.cloudflare.com)
2. **Wrangler CLI** - Already added to devDependencies
3. **Domain** - Your Cloudflare-managed domain (or use a temporary domain)

## Initial Setup

### 1. Install Dependencies
```bash
npm install
# or
yarn install
```

### 2. Authenticate with Cloudflare
```bash
npm run wrangler -- login
```
This opens a browser to authorize Wrangler with your Cloudflare account.

### 3. Update wrangler.toml
Edit `wrangler.toml` and replace:
- `example.com` with your actual domain
- `your-zone-id` with your Cloudflare Zone ID

To find your Zone ID:
1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Select your domain
3. Copy the Zone ID from the right sidebar

### 4. Configure Environment Variables
Create `.env.local` for development:
```bash
# .env.local
SENDGRID_API_KEY=your-key-here
# Add other environment variables
```

## Available Commands

### Development
```bash
npm run dev        # Local development
```

### Deployment
```bash
npm run deploy     # Deploy to production
npm run deploy:staging  # Deploy to staging environment
npm run cf-build   # Build with Cloudflare settings
```

## Cloudflare Services (Optional)

The following services can be enabled as needed:

### KV Storage (Key-Value Cache)
```toml
[[kv_namespaces]]
binding = "KV_NAMESPACE"
id = "your-kv-namespace-id"
preview_id = "your-preview-kv-namespace-id"
```

### D1 Database
```toml
[[d1_databases]]
binding = "DB"
database_name = "logoipsum-db"
database_id = "your-database-id"
```

### R2 Object Storage
```toml
[[r2_buckets]]
binding = "BUCKET"
bucket_name = "logoipsum-bucket"
```

### Durable Objects (Stateful Compute)
```toml
[[durable_objects.bindings]]
name = "DO_NAME"
class_name = "DurableObjectClassName"
```

## Troubleshooting

### Build Issues
- Ensure Node.js version is compatible (12+)
- Clear `.next/` and `.wrangler/` directories
- Run `npm install` again

### Authentication Errors
- Run `npm run wrangler -- logout` then login again
- Check Cloudflare account permissions

### Deployment Errors
- Verify Zone ID and domain are correct
- Check that your domain is added to Cloudflare
- Review deployment logs in Cloudflare Dashboard

## Project Structure with Cloudflare

```
.
├── pages/          # Next.js pages (routes)
├── components/     # React components
├── public/         # Static assets
├── wrangler.toml   # Cloudflare configuration
├── next.config.js  # Next.js config (updated for CF)
└── package.json    # Dependencies & scripts
```

## Performance Optimization

Cloudflare provides:
- Global CDN for fast content delivery
- Automatic caching
- DDoS protection
- URL routing and rewriting
- API Gateway features

## Security Best Practices

1. Never commit `.env.local` or sensitive keys
2. Use Cloudflare's Environment Secrets for sensitive data
3. Enable WAF (Web Application Firewall) rules
4. Use Cloudflare's authentication services

## Additional Resources

- [Wrangler Documentation](https://developers.cloudflare.com/workers/wrangler/)
- [Next.js on Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/nextjs/)
- [Cloudflare Workers](https://workers.cloudflare.com/)
- [API Monetization](https://developers.cloudflare.com/workers/platform/pricing/)
