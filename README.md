# Okteto Variables with Docker Compose Examples

This repository demonstrates different approaches to using **Okteto Variables** for configuring and parameterizing Docker Compose applications in cloud-native environments.

## Overview

Configuration is a core concept of every application, we understand how important it is for developers and teams to parametrize their project deployments and Okteto manifests. At Okteto we built different ways for you and your teams to manage and share config values using Okteto Variables.

Each example in this repository uses the same simple Go web application that displays a personalized greeting using environment variables (`MY_NAME` and `MY_COLOR`), but demonstrates different methods of providing these variables.

[Okteto Variables](https://www.okteto.com/docs/core/okteto-variables/) can be used during build time or run time.

## Examples

### 1. [Basic Docker Compose Variables](./docker-compose/)
**Use Case**: Direct parameterization of Docker Compose applications using Okteto Variables

- Shows how to use Okteto Admin Variables directly in Docker Compose
- Demonstrates environment variable substitution with default values
- Perfect for simple configuration scenarios

### 2. [Secret Manager Integration](./from-secret-manager/)
**Use Case**: Fetching secrets from external secret managers (Infisical)

- Integrates with Infisical secret manager
- Uses an Okteto Manifest to fetch secrets before deployment
- Ideal for production environments requiring centralized secret management

### 3. [Variable Manipulation](./manipulate-variables/)
**Use Case**: Transforming and manipulating variables before deployment

- Shows how to modify variable names and values during deployment
- Demonstrates custom deployment logic in Okteto Manifests
- Useful when variable names don't match between systems

## Documentation

- [Okteto Variables](https://www.okteto.com/docs/core/okteto-variables/)
- [Docker Compose Support](https://www.okteto.com/docs/reference/docker-compose/)
- [Admin Variables](https://www.okteto.com/docs/admin/dashboard/#admin-variables)
- [Okteto Manifest Reference](https://www.okteto.com/docs/reference/okteto-manifest/)

## Prerequisites

- Okteto CLI installed and configured
- Access to an Okteto instance
- Admin access for creating Okteto Variables (for examples 1 and 3)
- Infisical account and project setup (for example 2)