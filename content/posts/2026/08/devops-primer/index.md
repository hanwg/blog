---
title: "DevOps Tech Primer: Hands-On Learning with Sandboxes"
summary: "Hands-on DevOps learning experience with zero-setup"
date: 2026-08-31
draft: true
tags:
  - InfrastructureEngineering
cover:
   image: "devops-primer"
   alt: "Explore, experiment and learn within the sandbox"
   caption: "Explore, experiment and learn within the sandbox"
---

I recently led a tech primer to introduce DevOps tools which included Terraform, Docker and Kubernetes.
My goal was to deliver an engaging and interactive session where participants could easily grasp key concepts through hands-on exercises.

> [!Note]
These tech primers are designed to give the participants quick exposure to core DevOps tools.
The focus was on building fundamental concepts and workflows quickly through a hands-on session, rather than building enterprise-grade, production-ready systems right out of the gate.

## The Training Approach: DevOps Playgrounds

One of the biggest hurdles in conducting technical workshops is the environment setup.
Spending hours installing tools, updating dependencies and troubleshooting configuration issues can drain time and energy even before the training begins.

To eliminate this friction, I leveraged on the following sandboxes:
- [Terraform Sandbox](https://developer.hashicorp.com/terraform/tutorials/sandbox/sandbox)
- [Killercoda](https://killercoda.com/v)

These sandbox environments run entirely in the browser and allows participants to dive straight into the hands-on exercises without any setup.
Best of all, they are complete free and require no sign-ups!

## Module 1: Infrastructure-as-Code (IaC) with Terraform

I kicked off the session to show the modern method of provisioning infrastructure through code using Terraform.

To keep thing stress-free, HashiCorp's Terraform Sandbox uses [LocalStack](https://www.localstack.cloud/) to emulate the AWS cloud environments locally.
This meant no cloud accounts to configure, no risk of accidental billing, and the complete freedom to experiment.
After introducing core concepts, I walked through the Terraform `init`-`plan`-`apply` workflow.

## Module 2: Containerization with Docker

Next, I moved from infrastructure provisioning to packaging applications using Docker on Killercoda.
Once the participants understood *where* apps run, I focused on *how* apps are containerized.

I walked the participants through the full lifecycle of the container step-by-step:
1. Build an image by writing a simple `Dockerfile` and compiling it with `docker build`.
2. Run the image on a container using `docker run`.
3. Inspecting a running container with `docker inspect` to view container metadata, networking settings and environment variables.
4. Troubleshooting with `docker logs` to view output and diagnose runtime issues.
5. Tear down and clean up with the `docker stop` and `docker rm` commands respectively to keep the environment clean.

## Module 3: Container Orchestration with Kubernetes

After the participants gain an understanding of how containers operate individually, I introduced how Kubernetes manages, scales, and self-heals application deployments automatically.
