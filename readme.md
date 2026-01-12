Self-Hosted Supabase (Customized Deployment)

This repository contains a self-hosted Supabase stack with a customized docker-compose and environment configuration designed for real-world backend and infrastructure use cases.

It allows you to run all Supabase services locally or on any VM (EC2, on-prem, VPS, etc.) with controlled networking, authentication, storage, and observability.

This setup is based on the official Supabase self-hosting project but has been modified, hardened, and adapted for production-style environments.

What this repository provides

This repo spins up the complete Supabase platform, including:

PostgreSQL (with Supabase extensions)

GoTrue (Auth)

PostgREST (REST API)

Realtime

Storage

Kong (API Gateway)

Supabase Studio (Admin UI)

Analytics & logging services

Optional Vector / OTEL stack (depending on your deployment)

All services are started using Docker Compose with controlled ports, volumes, secrets, and networking.

Why this repository exists

The official Supabase repo is built to be generic.
This repository is built to be usable in real infrastructure.

It is designed for:

Hosting Supabase on your own server (EC2, VPS, on-prem)

Running Supabase inside private networks

Integrating Supabase into an existing backend stack

Custom domains, custom auth, and custom storage

Observability, logs, and data control

In short:

This repo is Supabase as infrastructure, not just a demo.


Repository structure
.
├── docker-compose.yml     # Modified Supabase stack
├── .env                   # Environment variables for all services (not pushed)
├── volumes/               # Persistent data
├── config/                # Supabase & Postgres configs
└── README.md

All changes from the upstream Supabase repository are focused on:

Stability
Clarity
Operability
Production-style defaults


Prerequisites

You need:

Docker
Docker Compose
At least 4GB RAM (8GB recommended)
Ports 3000, 5432, 8000, 4000 (or as defined in .env) available

Accessing Supabase

After startup:

Service	URL
Supabase Studio	http://localhost:3000

API Gateway (Kong)	http://localhost:8000

Postgres	localhost:5432

Login credentials are defined in .env.

What is different from the official Supabase repo

This repo is not a direct copy. It includes:

Customized docker-compose networking

Adjusted port mappings

Environment-specific .env structure

Easier VM / cloud deployment

Clean separation of secrets and configs

Ready for observability and log pipelines

It is designed to be:

Fork-safe, migration-friendly, and infra-ready.