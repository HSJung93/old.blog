---
title: "Building Java containers with Jib"
date: 2022-08-30T016:16:00+09:00
categories:
  - Memo
tags:
  - container
  - Jib
  - google cloud
# header:
#   teaser: /assets/images/code.jpg
---

Jib builds containers without using a Dockerfile or requiring a Docker installation. You can use Jib in the Jib plugins for Maven or Gradle, or you can use the Jib Java library.

## What does Jib do?

Jib handles all steps of packaging your application into a container image. You don't need to know best practices for creating Dockerfiles or have Docker installed.

Jib organizes your application into distinct layers; dependencies, resources, and classes; and utilizes Docker image layer caching to keep builds fast by only rebuilding changes. Jib's layer organization and small base image keeps overall image size small which improves performance and portability.