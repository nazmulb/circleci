# Circle CI

## What is CI/CD? 

**Continuous Integration (CI)** - is the practice of merging all developer working copies to a shared mainline several times a day. A complementary practice to CI is that before submitting work, each programmer must do a complete build and run (and pass) all unit tests. Integration tests are usually run automatically on a CI server (Jenkins/Drone/CercleCI) when it detects a new commit.

**Continuous Delivery (CD)** - It aims at building, testing, and releasing software with greater speed and frequency. Delivery team -> Version Control -> Build & unit tests -> Automated acceptance tests (e2e) -> UAT -> Release (Deployment, DevOps) 

<img alt="CI/CD Process" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/cicd.png" width="850px" />

## What is CircleCI?

CircleCI is Continuous Integration, a development practice which is being used by software teams allowing them to build, test and deploy applications easier and quicker on multiple platforms.

CircleCI uses a <a href="https://circleci.com/docs/2.0/writing-yaml/#section=configuration">YAML</a> (`.circleci/config.yml`) configuration file:

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:7.10
    steps:
      - checkout
      - run: echo "A first hello"
```

## Why Choose CircleCI?

#### Build Isolation
- Each build runs in a clean LXC Container, so you can be confident you’re getting a fresh run of your tests.
- SSH directly to the containers running your build for advanced debugging.
- Full scripting capabilities, including sudo.

#### Developer-Driven Configuration
- Build configuration lives in code via a `circle.yml` file, allowing each developer to tweak builds on a per-branch basis when needed.
- No centralized plugins to manage, so no bottlenecks on DevOps needing to configure each change to your builds.
- Store your secrets securely in env vars that will automatically be made available to your builds.

#### Fast Builds
- Parallelism allows splitting your tests across any number of containers, each of which runs as completely clean build. Tests can be automatically split based on timing distribution, or you can manually configure your test splits.
- A wide array of packages are pre-installed on the build containers and are ready to go, including most popular databases, languages, and frameworks.
- Dependency caching saves time on subsequent builds.

#### Broad, Extensible Coverage
- You do not need any dedicated server to run CircleCI
- First-class support for Docker builds
- First-class support for iOS builds
- Extensive API for custom integrations
- Automated inference gets most projects building with little or no configuration, but you are unrestricted in what languages, frameworks, and dependencies you can use in your builds.

#### GitHub Friendly
- Direct integration with GitHub for authentication and authorization, so you don’t need to reprovision accounts for your team.
- Automatic creation of hooks make it a breeze to get your repos set up to build when anyone on your team commits code.
- Quickly see the build status of your pull requests, and easily configure triggering builds when tagging your repo.

#### Knowledgeable Support
- <a href="https://discuss.circleci.com/">Active community</a> forums provide access to other developers with similar environments.

## How to set up CircleCI for CI/CD Pipeline?

### Step 1 - Creating a Repository:

Navigate to your account on GitHub.com and create a new repo <a href="https://github.com/nazmulb/circleci-101">circleci-101</a> with a README file.

### Step 2 - Adding a .yml File:

Please create a folder `.circleci` and add a YAML file `config.yml`. Please add the following contents in `.circleci/config.yml`:

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:7.10
    steps:
      - checkout
      - run: echo "A first hello"
```

Please commit and push this changes in your new repo `circleci-101`.

### Step 3 - Setting up Your Build on CircleCI:

- For this step, you will need a CircleCI account. Visit the CircleCI <a href="https://circleci.com/signup">signup page</a> and click “Sign Up with GitHub”. You will need to give CircleCI access to your GitHub account to run your builds. If you already have a CircleCI account then you can navigate to your <a href="https://circleci.com/dashboard">dashboard</a>.
- Next, you need to add you repo as a new project on CircleCI. To add your new repo, ensure that your GitHub account is selected in the dropdown in the upper-left, find the repository you just created below, and click the **Setup project** button next to it.
<img alt="Add Project" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/add-project.png" width="850px" />

- On the next screen, you’re given some options for configuring your project on CircleCI. Leave everything as-is for now and just click the **Start building** button a bit down the page on the right. 
<img alt="Add Project" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/add-project2.png" width="850px" />
<img alt="Add Project" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/add-project3.png" width="850px" />

### Step 4 - Running Your First CircleCI Build!:

You should see your build start to run automatically—and pass! So, what just happened? Click on the green Success button on the CircleCI dashboard to investigate the following parts of the run:

- **Spin up environment:** CircleCI used the `circleci/node:7.10` Docker image to launch a virtual computing environment.
- **Checkout code:** CircleCI checked out your GitHub repository and “cloned” it into the virtual environment launched in Step 1 from the `config.yml`.
- **echo:** This was the only other instruction in your `config.yml` file: CircleCI ran the `echo` command with the input “A first hello”.

<img alt="Running Your First CircleCI Build" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/build-pass.png" width="850px" />