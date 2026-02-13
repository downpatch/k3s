# Downpatch k3s Deployment

This repository contains the Kubernetes (k3s) configuration for running **downpatch.com**.

The goal of this repo is:

- Keep infrastructure public and reproducible
- Keep everything simple
- Make deployment easy to manage
- Stay aligned with the open source nature of the app

## Why k3s?

I chose **k3s** because:

- It is lightweight
- It is simple to install on a single VPS
- It gives proper Kubernetes features without full complexity
- It makes long-term management easier

Instead of manually running Docker containers and restarting them, k3s allows:

- Controlled deployments
- Rolling updates
- Health checks
- Automatic restarts
- TLS management with cert-manager
- Clean separation between app and infrastructure

It keeps things organised and reproducible.

## What This Repo Does

This repo contains:

- Deployment configuration for `downpatch/downpatch-web`
- Service definition
- Ingress configuration
- TLS via Let's Encrypt (cert-manager)
- Redirect from `www.downpatch.com` to `downpatch.com`

Everything here can be recreated on a fresh server.

## How It Works

1. Docker image is built and pushed to Docker Hub.
2. k3s runs the Deployment using that image.
3. Ingress exposes the app on port 80/443.
4. cert-manager automatically requests TLS certificates.


## Deployment

The main deployment:

- Runs `downpatch/downpatch-web`
- Pulls image from Docker Hub
- Uses `imagePullPolicy: Always`
- Has readiness + liveness probes
- Sets required environment variables

Updating the pod is simple:
`kubectl -n downpatch rollout restart deployment/downpatch-web`

## TLS

TLS is handled by:

- cert-manager
- HTTP-01 challenge
- ClusterIssuer `letsencrypt-http01`

## Why Public Infrastructure?

The application is open source.
The infrastructure should also be transparent and reproducible.

Anyone can:

- Inspect how it is deployed
- Recreate the setup
- Suggest improvements
