# Overview

# What is MagicFlow?

**MagicFlow** is a Python-based automation framework designed to streamline **idempotent daily operations** and reduce the overhead of **Business-As-Usual (BAU)** tasks. It is built to integrate seamlessly into any API-driven self-service platform, providing flexible and scalable automation capabilities.

This documentation provides:

- A general overview of MagicFlow's capabilities
- An introduction to the **Jobs** and **Workflow** frameworks that power its automation logic

---

## 🧠 Core Concepts

MagicFlow is structured around two primary components:

### Jobs

A **job** is a single, atomic task. Jobs can be composed into workflows to execute higher-order automation processes. Examples of job tasks include:

- Creating a pull request in GitLab
- Verifying approval status before proceeding
- Deploying resources in cloud environments (e.g., GCP, AWS)
- Managing secrets in systems like Kubernetes, Vault, or AWS SSM

### Workflows

A **workflow** is a sequence of jobs that represent a complete operational process. Workflows enable structured automation of common scenarios such as:

- Tenant provisioning
- Resource quota adjustments or scaling
- Backup validation during disaster recovery (DR) testing
- Invoking DR procedures
- Generating and collecting reports

---

## 🔄 Messaging with Kafka

MagicFlow uses **Apache Kafka** as its messaging backbone. It:

- **Consumes** job definitions and execution requests via Kafka topics
- **Publishes** job execution statuses and workflow outcomes back to Kafka

This decoupled architecture enables event-driven automation and seamless integration with external orchestration systems or service portals.

---

## 🔗 Integration

MagicFlow is designed with modularity in mind. It can be:

- Embedded into CI/CD pipelines
- Triggered via REST APIs or Kafka events
- Used in conjunction with infrastructure-as-code or service management platforms

---

## 📘 Next Steps

Continue exploring the documentation to learn more about:

- [Job Definitions](./jobs.md)
- [Workflow Configuration](./framework/components.md)
- [Platform Integration](./integration/architecture.md)
- [Examples & Use Cases](./examples.md)
