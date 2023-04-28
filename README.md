# Uffizzi Quickstart (~ 1 minute)

Go from pull request to Uffizzi Preview Environment in less than one minute...

### 1. Fork this repo

⚠️ Be sure to <ins>**uncheck**</ins> the option **Copy the `main` branch only**.  

This ensures that the `try-uffizzi` branch will be included in your fork.  
&nbsp;  
<img src="https://user-images.githubusercontent.com/7218230/191072997-94fdc9cc-2be2-4b44-900f-d4507c6df8a6.png" width="400">

### 2. Enable GitHub Actions workflows for your fork

Select **Actions**, then select **I understand my workflows, go ahead and enable them**.

<img src="https://user-images.githubusercontent.com/7218230/191074124-8ace8e9f-4970-46e5-9418-0f18d30bd08c.png" width="400">

### 3. Open a pull request for `try-uffizzi` branch against `main` in your fork

⚠️ Be sure that you're opening a PR on the branches of _your fork_ (i.e. `your-account/main` ← `your-account/try-uffizzi`).  

If you try to open a PR for `UffizziCloud/main` ← `your-account/try-uffizzi`, the Actions workflow will not run in this example.  

That's it! This will kick off a GitHub Actions workflow and post the Preview Environment URL as a comment to your PR issue.

<img alt="uffizzi-bot" src="https://user-images.githubusercontent.com/7218230/191825295-50422b35-23ac-47f6-8a22-c67f95c89d8c.png" width="400">

## What to expect

The PR will trigger a [GitHub Actions workflow](.github/workflows/uffizzi-preview.yaml) that creates a Uffizzi Preview Environment for the [microservices application](#architecture-of-this-example-app) defined by this repo. The Preview Environment URL will be posted as a comment in your PR issue when the workflow completes, along with a link to the Uffizzi Dashboard where you can view application logs. The Preview Environment will be deleted when the PR is merged/closed or after 1 hour ([configurable](https://github.com/UffizziCloud/quickstart/blob/6cbbb0f65e899cb05c96ea42531280f8d959df1d/docker-compose.uffizzi.yml#L7)).

## How it works

### Configuration

Preview Environments are configured with a [Docker Compose template](docker-compose.uffizzi.yml) that describes the application components and a [GitHub Actions workflow](.github/workflows/uffizzi-preview.yaml) that includes a series of jobs triggered by a `pull_request` event and subsequent `push` events:

1. [Build and push images to a container registry](https://github.com/UffizziCloud/quickstart/blob/6cbbb0f65e899cb05c96ea42531280f8d959df1d/.github/workflows/uffizzi-preview.yaml#L8-L114)
2. [Render a Docker Compose file](https://github.com/UffizziCloud/quickstart/blob/6cbbb0f65e899cb05c96ea42531280f8d959df1d/.github/workflows/uffizzi-preview.yaml#L116-L154) from the Docker Compose template and the built images
3. [Deploy the application to a Uffizzi Preview Environment](https://github.com/UffizziCloud/quickstart/blob/6cbbb0f65e899cb05c96ea42531280f8d959df1d/.github/workflows/uffizzi-preview.yaml#L156-L167) and post a comment to the PR issue
4. [Delete the Preview Environment](https://github.com/UffizziCloud/quickstart/blob/6cbbb0f65e899cb05c96ea42531280f8d959df1d/.github/workflows/uffizzi-preview.yaml#L169-L180) when the PR is merged/closed or after [`1h`](https://github.com/UffizziCloud/quickstart/blob/6cbbb0f65e899cb05c96ea42531280f8d959df1d/docker-compose.uffizzi.yml#L7)

### Uffizzi Cloud

Running this workflow will create a [Uffizzi Cloud](https://uffizzi.com) account and project from your GitHub user and repo information, respectively. If you sign in to the [Uffizzi Dashboard](https://app.uffizzi.com/sign_in) you can view logs, password protect your Preview Environments, manage projects and team members, set role-based access controls, and configure single-sign on (SSO).

Open-source projects preview for free on Uffizzi Cloud. All other accounts can subscribe to our Starter or Pro plans. See [our pricing](https://uffizzi.com/pricing) for details. Or if you're an open-source maintainer, you can request free access as by sending an email to opensource@uffizzi.com. Alternatively, if you don't want to use Uffizzi Cloud, you can [install open-source Uffizzi](https://github.com/UffizziCloud/uffizzi_app/blob/develop/INSTALL.md) on your own Kubernetes cluster.

## Acceptable Use

We strive to keep Uffizzi Cloud free or inexpensive for individuals and small teams. Therefore, activities such as crypto mining, filesharing, bots, and similar uses that lead to increased cost and intermittent issues for other users are strictly prohibited per the [Acceptable Use Policy](https://uffizzi.zendesk.com/hc/en-us/articles/4410657390999-Acceptable-Use-Policy). Violators of this policy are subject to permanent ban.

## Architecture of this Example App

The application defined by this repo allows users to vote for dogs or cats and see the results. It consists of the following microservices:

<img src="https://user-images.githubusercontent.com/7218230/192601868-562b705f-bf39-4eb8-a554-2a0738bd8ecf.png" width="400">

* **voting-app** - A frontend web app in [Python](/vote) which lets you vote between two options
* **redis** - A [Redis](https://hub.docker.com/_/redis/) queue which collects new votes
* **worker** - A [.NET Core](/worker/src/Worker) worker which consumes votes and stores them in…
* **db** - A [PostgreSQL](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* **result-app** - A [Node.js](/result) web app which shows the results of the voting in real time

## Set up Preview Environments for your application

You can follow this [step-by-step guide](https://docs.uffizzi.com/set-up-uffizzi-for-your-application) to configure Preview Environments for your own application. The required components are:

- **A Docker Compose template (`docker-compose.uffizzi.yml`) committed to your repo** - This template must include [`ingress`](https://github.com/UffizziCloud/quickstart/blob/6aba97b1e27c8fafba2d6461087abfe06becf9ce/docker-compose.uffizzi.yml#L2-L5) and `services` definitions. For a full list of supported keywords, see [Docker Compose for Uffizzi](https://docs.uffizzi.com/references/compose-spec/).

- **A Uffizzi preview job added to your pipeline** - In the example app used by this quickstart guide, we use GitHub and GitHub Actions, but Uffizzi is designed to work with any version control system or CI platform. As a convenience, we've written actions/jobs for [these popular CI platforms](https://github.com/UffizziCloud/uffizzi_app/tree/develop/ci/). If your platform is not listed, you can still add Uffizzi to your pipeline by wrapping the [Uffizzi CLI](https://github.com/UffizziCloud/uffizzi_cli), which is distributed as a container image [available on Docker Hub](https://hub.docker.com/r/uffizzi/cli). See our GitHub [`preview-action`](https://github.com/UffizziCloud/preview-action/blob/master/action.yaml) and [reusable workflow](https://github.com/UffizziCloud/preview-action/blob/master/.github/workflows/reusable.yaml) as examples.

## FAQs
<details><summary><b>What about my database?</b></summary>
<p>All services defined by your Docker Compose file are deployed to Preview Environments as containers—this includes databases, caches, and other datastores. This means that even if you use a managed database service like Amazon RDS for production, you should use a database <i>image</i> in your Compose (See <a href="https://github.com/UffizziCloud/quickstart/blob/6aba97b1e27c8fafba2d6461087abfe06becf9ce/docker-compose.uffizzi.yml#L15">this example</a> that uses a <code>postgres</code> image from Docker Hub).</p>

<p>If your application requires test data, you will need to seed your database when your Preview Environment is created. Here are two methods for seeding databases:</p>
<ol>
  <li>(Recommended) Have your application perform a data migration on start-up</li>
  <li>Bundle test data into the database image itself. This method is only recommended for small datasets (< 50MB), as it will increase the size of your image and deployment times.</li>
</ol>
</details>

<details><summary><b>Does Uffizzi support monorepos/polyrepos?</b></summary>
Yes. Your CI pipeline will typically include a series of <code>build</code>/<code>push</code> steps for each of the components of your application. Uffizzi just needs to know the fully qualified container registry URL for where to find these built images.
</details>

<details><summary><b>Does Uffizzi support &nbsp; _____________?</b></summary>
Uffizzi is container-centric and primarily designed for web languages. In general, if your application can be containerized, described with Docker Compose, and accepts HTTP traffic, Uffizzi can preview it.
</details>

<details><summary><b>How can my application services communicate?</b></summary>
Just like when you run <code>docker-compose up</code> locally, all the <code>services</code> defined in your Compose share a local network and can communicate via <code>localhost:port</code>. Applications that belong to different Preview Environments may only communicate via the public Internet.
</details>

<details><summary><b>How is Uffizzi different from GitHub Actions (or other CI providers)?</b></summary>
Uffizzi does not replace GitHub Actions or any other CI provider. Uffizzi previews are meant to be added as a step in your existing CI pipeline, after your container images are built and pushed to a container registry.
</details>

<details><summary><b>Can I connect Uffizzi with Netlify/Vercel?</b></summary>
Yes. While Uffizzi supports full-stack previews, some users who already leverage frontend platforms like <a href="https://www.netlify.com">Netlify</a> or <a href="https://vercel.com">Vercel</a> want to add Uffizzi previews for their APIs/backend. For help configuring this scenario see:
<ul>
  <li><a href="https://github.com/UffizziCloud/netlify-uffizzi-previews">Netlify + Uffizzi</a></li>
  <li><a href="https://github.com/UffizziCloud/foodadvisor">Vercel + Uffizzi</a></li>
</ul>
</details>

<details><summary><b>Is Uffizzi open source?</b></summary>
Yes. Check out the <a href="https://github.com/UffizziCloud/uffizzi_app">main repo</a>
</details>


## Get in touch

For questions, concerns, issues, or feature requests, please join our fast growing [community](https://uffizzi.slack.com/join/shared_invite/zt-ffr4o3x0-J~0yVT6qgFV~wmGm19Ux9A#/shared-invite/email) on Slack.
