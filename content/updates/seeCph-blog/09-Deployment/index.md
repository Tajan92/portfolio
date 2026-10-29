---
title: "seeCph: Deployment"
description: "Setting project up for deployment with GitHub Actions and Watchtower for automatic deployment when pushing project to main"
summary: "Deployment of the project, using GitHub Actions to run test and build maven project and Watchtower to poll for a new image to deploy."
date: 2026-10-25
project: "seecph"
header: "Deployment"
subheader: "25 october, 2026"
icon: "rss"
externalUrl: "blog/seecph-blog/#deployment"
weight: 992
categories: ["Deployment"]
tags: ["GitHub Actions", "Watchtower", "Docker Hub", "DigitalOcean"]
---

Finally time for deployment of my REST API, setting up automatic deployment when pushing to main. For this I use GitHub Actions and a Watchtower on DigitalOcean to poll for a new image on Docker Hub.

When pushing the project to GitHub it checks if it's push or pull request to main.

    Push:
        -  Runs test and Maven builds the project as a fat jar and build a Docker image contain the jar and pushes it to Docker Hub.

    Pull request:
        - Runs test and Maven builds the project as a fat jar, but nothing is deployed.

If a test fails, the process stops before the Docker steps, so a broken build never reaches the server.

On the DigitalOcean, Watchtower runs as a container next to the application. It polls Docker Hub for a new version of the image and, when one appears, pulls it and restarts the application container with the same settings from `docker-compose.yml`.

It's secured using secrets on both ends, the .env and GitHub secrets holds values needed for the process. GitHub holds values needed for the build like JWT settings used for tests and Docker Hub username and a secret access token for the push the image.
Everything needed when the application is running on Digital Ocean is kept in `.env`, so the image itself doesn’t contain any secrets.
