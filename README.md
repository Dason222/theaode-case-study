# AODE / Theaode — Self-Hosted Deployment Platform Case Study

AODE is a commercial self-hosted deployment portal for running Vercel-like deployments on your own VPS or server.

Install it once on a VPS, connect GitHub, add projects, map domains, manage environment variables, build Docker-based apps, and deploy from a dashboard you own. The source code is private because this is a paid product, but this case study documents the product idea, architecture, UI flow, and engineering decisions.

Website: https://theaode.com

## Product summary

AODE gives developers the developer experience of platforms like Vercel or Netlify, but on infrastructure they control.

Instead of manually SSH-ing into a server, writing reverse proxy configs, setting up SSL, managing Docker networks, and restarting services by hand, AODE turns a VPS into a small deployment platform.

## Project status

- Product type: Commercial self-hosted deployment platform
- Source code: Private
- Public proof: Case study, screenshots, architecture, and demo notes
- Main purpose: Deploy GitHub projects to your own VPS with less manual server work
- Target users: Solo builders, indie hackers, small teams, and developers who want control, lower platform costs, and self-hosting

## What I built

- Public landing page and product positioning
- VPS installation flow
- Developer dashboard
- Project management UI
- GitHub-based deployment flow
- Domain management flow
- Automatic SSL concept with Let's Encrypt
- Docker-based deployment structure
- Reverse proxy setup concept with Traefik
- Environment variable management
- Deployment logs view
- Deployment history / rollback concept
- Documentation and onboarding flow
- Commercial product packaging and pricing flow

## Problem

Self-hosting gives control and lower recurring costs, but normal VPS deployment is painful.

Common problems:

- Manual SSH deployments
- Repeated server setup
- Reverse proxy and SSL configuration
- No simple rollback strategy
- Multiple projects on one server becoming messy
- No clean dashboard for deployments
- No simple Git push-to-deploy flow
- No central place for domains, env vars, logs, and project status

## Solution

AODE turns a VPS into a deployment panel.

The intended flow:

```text
Install AODE on VPS
  ↓
Open web dashboard
  ↓
Connect GitHub repository
  ↓
Add project
  ↓
Configure domain and environment variables
  ↓
Deploy
  ↓
Auto-SSL + Docker + reverse proxy handle the infrastructure
```

## Main features

| Feature | Description |
|---|---|
| Git-based deploys | Deploy from GitHub repositories and support push-to-deploy workflows |
| Dashboard | Manage projects, domains, deployments, logs, and settings in one panel |
| Docker isolation | Run apps in containers instead of manually managing processes |
| Reverse proxy | Route projects through a proxy layer such as Traefik |
| Automatic SSL | Use Let's Encrypt certificates for custom domains |
| Environment variables | Manage project secrets and configuration from the panel |
| Build logs | Show deployment output and errors in real time |
| Rollbacks | Keep deployment history and allow reverting broken deployments |
| Multi-project VPS | Run several apps on one VPS without port/config chaos |
| Self-hosted control | Keep root access, predictable cost, and data ownership |

## Architecture overview

```text
Developer
  ↓ git push / deploy action
GitHub Repository
  ↓ webhook / deploy request
AODE Dashboard
  ↓
Build System
  ↓
Docker Image / Container
  ↓
Traefik Reverse Proxy
  ↓
Let's Encrypt SSL
  ↓
Public Domain
  ↓
Running App on User VPS
```

## Deployment flow

```text
1. User installs AODE on Ubuntu/Debian VPS
2. Installer prepares Docker, proxy, SSL, and dashboard services
3. User opens dashboard in browser
4. User connects GitHub access
5. User adds a repository as a project
6. AODE builds the app using Docker/Nixpacks/custom build config
7. AODE creates/runs the container
8. Traefik routes the domain to the app
9. Let's Encrypt provisions SSL
10. Future git pushes can trigger redeploys
```

## Product positioning

AODE is positioned between manual VPS deployment and managed platforms.

| Option | Strength | Weakness |
|---|---|---|
| Manual VPS | Cheap, full control | Slow setup, manual deploys, SSL/proxy work |
| Vercel / Netlify | Great developer experience | Platform limits, recurring costs, less server control |
| AODE | Vercel-like flow on your own VPS | User still owns server responsibility |

## What this project demonstrates

This case study demonstrates full-stack product engineering across UI, backend, infrastructure, and product packaging.

It shows experience with:

- Full-stack dashboard design
- Deployment platform architecture
- Docker-based application deployment
- Reverse proxy and SSL workflows
- GitHub integration planning
- VPS/self-hosted infrastructure
- Environment variable and secrets management concepts
- Build logs and deployment history UX
- Commercial product positioning
- Documentation and onboarding

## Why the source code is private

AODE is a paid commercial product. This repository only documents the product, architecture, and engineering decisions. Source code, license logic, commercial assets, private implementation details, and production secrets are not included.

## Screenshots

Screenshots can be added in the `/screenshots` folder.

Suggested screenshots:

1. Landing page
2. Project dashboard
3. Deployment logs
4. Domain management
5. Environment variables
6. Install/onboarding flow
7. Pricing page

## Demo

A demo video or private walkthrough can be added in the `/demo` folder.

Suggested demo flow:

1. Show the landing page
2. Explain the VPS/self-hosted deployment problem
3. Show the dashboard
4. Add a GitHub project
5. Configure domain and env vars
6. Run a deployment
7. Show logs
8. Explain Docker, Traefik, SSL, and rollback flow at a high level

## Related portfolio projects

AODE fits together with my other work:

- Full-stack e-commerce audit platform
- AI automation workflows with n8n
- Trading and analytics dashboards
- Self-hosted infrastructure and deployment experiments
- AI/backend systems with real operational use cases

Together, these projects show my focus on building complete systems: frontend, backend, database, automation, deployment, infrastructure, and product thinking.
