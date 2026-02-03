<p align="center">
  <a href="https://unbyte.de">
    <img src="https://www.unbyte.de/wp-content/uploads/2024/12/unbyte_logo.svg" alt="unbyte GmbH" width="300">
  </a>
</p>

# PowerDNS Helm Chart

[![PowerDNS](https://img.shields.io/badge/PowerDNS-4.9.x-002B5C)](https://www.powerdns.com/)
[![Helm](https://img.shields.io/badge/Helm-3.x-0f1689.svg)](https://helm.sh)
[![GHCR](https://img.shields.io/badge/GHCR-ghcr.io-181717?logo=github&logoColor=white&label=chart)](https://github.com/orgs/unbyte-de/packages?repo_name=powerdns-chart)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.31+-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io)
<!-- [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE) -->
<!-- [![GitHub release](https://img.shields.io/github/v/release/unbyte/powerdns-chart)](https://github.com/unbyte/powerdns-chart/releases) -->
<!-- [![GitHub Actions](https://img.shields.io/github/actions/workflow/status/unbyte/powerdns-chart/lint.yaml?label=lint)](https://github.com/unbyte/powerdns-chart/actions/workflows/lint.yaml) -->
<!-- [![Pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit) -->
<!-- [![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md) -->

A Helm chart for deploying [PowerDNS](https://www.powerdns.com/) Authoritative Server and Recursor on Kubernetes.

## Overview

This chart deploys a complete PowerDNS stack consisting of:

- **PowerDNS Authoritative Server** - Serves DNS records for your internal/private domains
- **PowerDNS Recursor** - A recursive DNS resolver that forwards queries to the authoritative server for internal domains and to external resolvers (e.g., Google DNS) for everything else

This setup is ideal for Kubernetes environments requiring internal DNS resolution with the ability to manage custom DNS zones.

## Features

- 🔐 Secure by default with non-root containers
- 📦 SQLite backend for the authoritative server
- 🔄 Support for DNS zone files
- 🌐 REST API enabled for both auth and recursor
- 📊 Compatible with [ExternalDNS](https://github.com/kubernetes-sigs/external-dns) via DNS updates
- 💾 Persistent storage for DNS database
- ⚖️ LoadBalancer service for recursor (configurable)

## Prerequisites

- Kubernetes 1.31+
- Helm 3.x
- A Kubernetes secret containing API keys for PowerDNS

## Installation

### 1. Create the API key secret

```bash
kubectl create secret generic pdns-api-key \
  --from-literal=PDNS_AUTH_API_KEY='your-auth-api-key' \
  --from-literal=PDNS_RECURSOR_API_KEY='your-recursor-api-key'
```

### 2. Install the chart

```bash
helm upgrade --install powerdns \
  oci://ghcr.io/unbyte-de/powerdns-chart/powerdns \
  --version 0.4.0 \
  --namespace powerdns \
  --create-namespace
```

Or install directly from this repository:

```bash
helm install powerdns ./powerdns
```

## Local Developmemt

To run yamlfmt locally:

```sh
yamlfmt "**/*.{yaml,yml,yamlfmt}"
# Or remove -quiet and/or -lint flag of yamlfmt in .pre-commit-config.yaml and run
pre-commit run --all-files
```
