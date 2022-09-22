# Uffizzi Quickstart (~ 1 minute)

Get started using Uffizzi Preview Environments in 3 simple steps...

### 1. Fork this repo  

Be sure to uncheck the option **Copy the `main` branch only**. This ensures that the `try-uffizzi` branch will be included in your fork.  

<img src="https://user-images.githubusercontent.com/7218230/191072997-94fdc9cc-2be2-4b44-900f-d4507c6df8a6.png" width="600">  

### 2. Enable GitHub Actions workflows for your fork

Select **Actions**, then select **I understand my workflows, go ahead and enable them**. GitHub Actions is free in public repositories.   

<img src="https://user-images.githubusercontent.com/7218230/191074124-8ace8e9f-4970-46e5-9418-0f18d30bd08c.png" width="600">  

### 3. Open a pull request for `try-uffizzi` branch against `main` in your fork  

That's it! This will kick off a GitHub Actions workflow and post the Preview Environment URL as a comment to your PR issue.  

## What to expect  

The PR will trigger a [GitHub Actions workflow](https://github.com/UffizziCloud/quickstart/blob/main/.github/workflows/uffizzi-environment.yml) that creates a Uffizzi Preview Environment for the [microservices application](#architecture-of-this-example-app) defined by this repo. The Preview Environment URL will be posted as a comment in your PR issue when the workflow completes, along with a link to the Uffizzi Dashboard where you can view application logs. The Preview Environment and comment is deleted after 1 hour or when the PR is merged/closed.  

[Screenshot of uffizzi-bot comment]

## How it works  

#### Configuration

Previews are configured with a [Docker Compose template](https://github.com/UffizziCloud/quickstart/blob/main/docker-compose.template.yml) that describes the application configuration and a [GitHub Actions workflow](https://github.com/UffizziCloud/quickstart/blob/main/.github/workflows/uffizzi-environment.yml) that includes a series of jobs triggered by a `pull_request` event and subsequent `push` events:  

1. [Build and push images to a container registry](https://github.com/UffizziCloud/quickstart/blob/a6d9ec7816da58c4d8c5b2ea47ad9cf3cfa0585f/.github/workflows/uffizzi-previews.yml#L14-L124)  
2. [Render a Docker Compose file](https://github.com/UffizziCloud/quickstart/blob/a6d9ec7816da58c4d8c5b2ea47ad9cf3cfa0585f/.github/workflows/uffizzi-previews.yml#L126-L164) from the Docker Compose template and the built images  
3. [Deploy the application (per the Docker Compose file) to a Uffizzi Preview Environment](https://github.com/UffizziCloud/quickstart/blob/a6d9ec7816da58c4d8c5b2ea47ad9cf3cfa0585f/.github/workflows/uffizzi-previews.yml#L166-L185) and post a comment to the PR issue  
4. [Delete the Preview Environment](https://github.com/UffizziCloud/quickstart/blob/a6d9ec7816da58c4d8c5b2ea47ad9cf3cfa0585f/.github/workflows/uffizzi-previews.yml#L187-L200) when the PR is merged/closed or after `1h`      

#### Uffizzi Cloud

Running this workflow will create a [Uffizzi Cloud](https://uffizzi.com) account and project from your GitHub user and repo information. If you sign in to the Uffizzi Dashboard you can view logs, password protect your Preview Environments, manage projects and team members, set role-based access controls, and configure single-sign on (SSO).

Each account receives 10,000 preview minutes per month for free. If you exceed this amount, your Preview Environments will be paused unless you add a credit card. See [our pricing](https://uffizzi.com/pricing) for details.

## Acceptable Use

We strive to keep Uffizzi Cloud free or inexpensive for individuals and small teams. Therefore, activities such as crypto mining, filesharing, bots, and similar uses that lead to increased cost and intermittent issues for other users are strictly prohibited per the [Acceptable Use Policy](https://uffizzi.zendesk.com/hc/en-us/articles/4410657390999-Acceptable-Use-Policy). Violators of this policy are subject to permanent ban.  

## Architecture of this Example App

This application allows users to vote for dogs or cats and see the results. It consists of the following microservices:  

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) or [ASP.NET Core](/vote/dotnet) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) or [NATS](https://hub.docker.com/_/nats/) queue which collects new votes
* A [.NET Core](/worker/src/Worker), [Java](/worker/src/main) or [.NET Core 2.1](/worker/dotnet) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) or [TiDB](https://hub.docker.com/r/dockersamples/tidb/tags/) database backed by a Docker volume
* A [Node.js](/result) or [ASP.NET Core SignalR](/result/dotnet) webapp which shows the results of the voting in real time


Notes
-----

This example application is meant to demonstrate Uffizzi capabilities and workflow. To configure Preview Environments for your own application, be sure to add a Docker Compose template and add a Uffizzi preview job to your pipeline. This uses Github and Github Actions, but Uffizzi is designed to work with any version control system, container registry, or [CI provider](https://github.com/UffizziCloud/uffizzi_app/tree/develop/ci/).  

You can configure Uffizzi to preview your APIs/backend while static site hosting platforms like [Netlify](https://www.netlify.com) or [Vercel](https://vercel.com/) preview your frontend. For help configuring this scenario see:  

- [Netlify + Uffizzi](https://github.com/UffizziCloud/netlify-uffizzi-previews)
- [Vercel + Uffizzi](https://github.com/UffizziCloud/foodadvisor)

For questions, concerns, issues, or feature requests, please join our fast growing [community](https://uffizzi.slack.com/join/shared_invite/zt-ffr4o3x0-J~0yVT6qgFV~wmGm19Ux9A#/shared-invite/email) on Slack.  
