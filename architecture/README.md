# AODE Architecture Notes

This document describes the public architecture overview for AODE / Theaode. It does not include private source code, license logic, production secrets, or commercial implementation details.

## High-level architecture

```text
Developer
  ↓
GitHub Repository
  ↓
AODE Dashboard
  ├─ Projects
  ├─ Deployments
  ├─ Domains
  ├─ Environment Variables
  ├─ Build Logs
  └─ Settings
  ↓
Build System
  ├─ Docker
  ├─ Nixpacks / buildpacks concept
  └─ Custom build config concept
  ↓
Runtime Layer
  ├─ Docker containers
  ├─ Docker networks
  └─ Health checks
  ↓
Routing Layer
  ├─ Traefik reverse proxy
  ├─ Custom domains
  └─ Let's Encrypt SSL
  ↓
Public Apps Running on User VPS
```

## Core idea

AODE turns a normal VPS into a developer-friendly deployment platform.

The user keeps control of the server, but AODE handles the boring deployment work: project setup, builds, containers, domains, SSL, logs, and redeploys.

## Main components

### 1. Web dashboard

The dashboard is the control panel for projects, domains, deployments, logs, environment variables, and settings.

### 2. GitHub integration

AODE connects to GitHub repositories so users can deploy projects without manually copying files to the server.

### 3. Build system

The build system prepares the application for production. Depending on project type, this can be based on Docker, Nixpacks/buildpacks, or custom build instructions.

### 4. Docker runtime

Applications run in containers so several projects can live on the same VPS without port and dependency conflicts.

### 5. Reverse proxy

A reverse proxy such as Traefik routes domains to the correct app containers.

### 6. SSL automation

Let's Encrypt certificates are used so users can connect custom domains with HTTPS without manual certificate work.

### 7. Deployment history

Deployments should keep useful metadata: status, commit, branch, logs, timestamp, and rollback target.

## Deployment flow

```text
1. User installs AODE on VPS
2. AODE prepares Docker, proxy, SSL, and dashboard services
3. User opens the dashboard
4. User connects GitHub
5. User creates a project from a repository
6. User configures domain and environment variables
7. AODE builds the app
8. AODE starts/replaces the container
9. Reverse proxy routes traffic
10. SSL is issued or renewed automatically
11. Logs and deployment status are visible in the dashboard
```

## Design goals

### Vercel-like workflow on self-hosted infrastructure

The experience should feel familiar: connect repo, configure project, deploy, view logs, roll back if needed.

### Server ownership

The user owns the VPS, data, pricing, and server access.

### Multi-project support

One VPS should be able to run many projects cleanly through containers, domains, and routing rules.

### Less manual server work

The user should not need to repeat nginx/Traefik config, SSL setup, Docker network setup, and manual restart steps for every app.

### Clear developer dashboard

The UI should show what matters: project status, domains, deployments, logs, environment variables, and actions.

## Security and privacy notes

Because AODE is a commercial product, this repository does not expose:

- Source code
- Private implementation details
- Environment variables
- License or payment logic
- Production infrastructure details
- Customer data
- GitHub tokens
- Server secrets
