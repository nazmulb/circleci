# Circle CI

## What is CI/CD? 

**Continuous Integration (CI)** - is the practice of merging all developer working copies to a shared mainline several times a day. A complementary practice to CI is that before submitting work, each programmer must do a complete build and run (and pass) all unit tests. Integration tests are usually run automatically on a CI server (Jenkins/Drone/CercleCI) when it detects a new commit.

**Continuous Delivery (CD)** - It aims at building, testing, and releasing software with greater speed and frequency. Delivery team -> Version Control -> Build & unit tests -> Automated acceptance tests (e2e) -> UAT -> Release (Deployment, DevOps) 

<img alt="CI/CD Process" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/cicd.png" width="950px" />

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

## How to set up CircleCI?

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
<img alt="Add Project" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/add-project.png" width="950px" />

- On the next screen, you’re given some options for configuring your project on CircleCI. Leave everything as-is for now and just click the **Start building** button a bit down the page on the right. 
<img alt="Add Project" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/add-project2.png" width="950px" />
<img alt="Add Project" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/add-project3.png" width="950px" />

### Step 4 - Running Your First CircleCI Build!:

You should see your build start to run automatically—and pass! So, what just happened? Click on the green Success button on the CircleCI dashboard to investigate the following parts of the run:

- **Spin up environment:** CircleCI used the `circleci/node:7.10` Docker image to launch a virtual computing environment.
- **Checkout code:** CircleCI checked out your GitHub repository and “cloned” it into the virtual environment launched in Step 1 from the `config.yml`.
- **echo:** This was the only other instruction in your `config.yml` file: CircleCI ran the `echo` command with the input “A first hello”.

<img alt="Running Your First CircleCI Build" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/build-pass.png" width="950px" />

### Step 5 - Breaking Your Build!:

Edit your `config.yml` file and replace `echo "A first hello"` with `notacommand`. Now commit and push your changes in Github repo. When you navigate back to the Builds page in CircleCI, you will see that a new build was triggered. This build will fail with a red Failed button and will send you a notification email of the failure.

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:7.10
    steps:
      - checkout
      - run: notacommand
```

<img alt="Breaking Your Build" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/build-failed.png" width="950px" />

## Concepts:

### Steps:

Steps are actions that need to be taken to perform your job. Steps are usually a collection of executable commands. For example, the `checkout` step checks out the source code for a job. Then, the `run` step executes the `make test` command using a non-login shell by default.

```yml
#...
    steps:
      - checkout # Special step to checkout your source code
      - run: # Run step to execute commands
          name: Running tests
          command: make test
#...     
```

### Image:

An image is a packaged system that has the instructions for creating a running container. The Primary Container is defined by the first image listed in `.circleci/config.yml` file. This is where commands are executed for jobs using the Docker executor.

For convenience, CircleCI maintains several Docker images. These images are typically extensions of official Docker images and include tools especially useful for CI/CD. All of these pre-built images are available in the <a href="https://hub.docker.com/r/circleci/">CircleCI org on Docker Hub</a>.

```yml
version 2
 jobs:
   build1: # job name
     docker: # Specifies the primary container image
       - image: buildpack-deps:trusty

       - image: postgres:9.4.1 # Specifies the database image
        # for the secondary or service container run in a common
        # network where ports exposed on the primary container are
        # available on localhost.
         environment: # Specifies the POSTGRES_USER auth environment variable
           POSTGRES_USER: root
...
   build2:
     machine: # Specifies a machine image that uses
     # an Ubuntu version 14.04 image with Docker 17.06.1-ce
     # and docker-compose 1.14.0, follow CircleCI Discuss Announcements
     # for new image releases.
       image: circleci/classic:201708-01
...       
   build3:
     macos: # Specifies a macOS virtual machine with Xcode version 9.0
       xcode: "9.0"       
...          
```

### Jobs:

Jobs are a collection of steps. CircleCI enables you to run jobs in one of three environments:

- Within Docker images (`docker`)
- Within a Linux virtual machine (VM) image (`machine`)
- Within a macOS VM image (`macos`)

Machine includes a default image if not specified, for Docker and macOS, you must also declare an image.

#### Using Docker:

Using `docker` executor to build a Docker image requires <a href="https://circleci.com/docs/2.0/building-docker-images/">Remote Docker</a>

```yml
jobs:
  build:
    docker:
      - image: buildpack-deps:trusty
```

#### Using Machine:

Using the `machine` executor gives your application full access to OS resources and provides you with full control over the job environment. This control can be useful in situations where you need to use `ping` or modify the system with `sysctl` commands.

Using the `machine` executor also enables you to build a Docker image without downloading additional packages for languages like Ruby and PHP.

```yml
version: 2
jobs:
  build:
    machine: true
```

The default image for the machine executor is `circleci/classic:latest`. You can specify other images by using the `image` key.

```yml
version: 2
jobs:
  build:
    machine:
      image: circleci/classic:2017-01  # pins image to specific version using YYYY-MM format
```
#### Using macOS:

Using the `macos` executor allows you to run your job in a macOS environment on a VM. You can also specify which version of Xcode should be used.

```yml
jobs:
  build:
    macos:
      xcode: "9.0"  
    steps:
      # Commands will execute in macOS container
      # with Xcode 9.0 installed
      - run: xcodebuild -version
```

You can <a href="https://circleci.com/docs/2.0/executor-types/">read more</a>.

### Workflows:

Workflows define a list of jobs and their run order. It is possible to run jobs in parallel, sequentially, on a schedule, or with a manual gate using an approval job.

<img alt="Workflows" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/workflows.png" width="950px" />

```yml
version: 2
jobs:
  one:
    docker:
      - image: circleci/node:7.10
    steps:
      - checkout
      - run: echo "A first hello"
      - run: mkdir -p my_workspace
      - run: echo "Trying out workspaces" > my_workspace/echo-output
      - persist_to_workspace:
          # Must be an absolute path, or relative path from working_directory
          root: my_workspace
          # Must be relative path from root
          paths:
            - echo-output      
  two:
    docker:
      - image: circleci/node:7.10
    steps:
      - checkout
      - run: echo "A more familiar hi"  
      - attach_workspace:
          # Must be absolute path or relative path from working_directory
          at: my_workspace
      - run: 
          name: Compare the result
          command: |
            if [[ $(cat my_workspace/echo-output) == "Trying out workspaces" ]]; then
              echo "It worked!";
            else
              echo "Nope!"; exit 1
            fi
workflows:
  version: 2
  one_and_two:
    jobs:
      - one
      - two:
          requires:
            - one
```

### Cache:

Caching lets you reuse the data from expensive fetch operations from previous jobs. After the initial job run, future instances of the job will run faster by not redoing work. A prime example is package dependency managers such as Npm, Yarn, or Pip.

<img alt="Cache" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/Diagram-v3-Cache.png" width="950px" />

```yml
version: 2.1
executors:
  node:
    docker:
      - image: 'circleci/node:8'
    shell: /bin/bash
    working_directory: ~/app
jobs:
  build:
    executor: node
    steps:
      - checkout
      - restore_cache:
          keys:
            # Find a cache corresponding to this specific package-lock.json checksum
            # when this file is changed, this key will fail
            - v1-npm-deps-{{ checksum "package-lock.json" }}
            # Find the most recently generated cache used from any branch
            - v1-npm-deps-
      - run:
          name: Install Node.js dependencies with Npm
          command: npm install
      - save_cache:
          paths:
            - ./node_modules
          key: v1-npm-deps-{{ checksum "package-lock.json" }}
```

### Workspaces:

When a Workspace is declared in a job, one or more files or directories can be added. Each addition creates a new layer in the Workspace filesystem. Downstreams jobs can then use this Workspace for its own needs or add more layers on top.

Unlike caching, Workspaces are not shared between runs as they no longer exists once a Workflow is complete. 

<img alt="Workspaces" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/Diagram-v3-Workspaces.png" width="950px" />

```yml
version: 2.1
executors:
  node:
    docker:
      - image: 'circleci/node:8'
    shell: /bin/bash
    working_directory: ~/app
jobs:
  build:
    executor: node
    steps:
      - checkout
      - restore_cache:
          keys:
            # Find a cache corresponding to this specific package-lock.json checksum
            # when this file is changed, this key will fail
            - v1-npm-deps-{{ checksum "package-lock.json" }}
            # Find the most recently generated cache used from any branch
            - v1-npm-deps-
      - run:
          name: Install Node.js dependencies with Npm
          command: npm install
      - save_cache:
          paths:
            - ./node_modules
          key: v1-npm-deps-{{ checksum "package-lock.json" }}
      - persist_to_workspace:
          root: ~/app
          paths:
            - .
  test:
    executor: node
    steps:
      - attach_workspace:
          at: ~/app
      - run:
          name: Test
          command: npm test
workflows:
  version: 2
  build_and_test:
    jobs:
      - build
      - test:
          requires:
            - build
```

### Artifacts:

Artifacts persist data after a workflow is completed and may be used for longer-term storage of the outputs of your build process. For instance if you have a Java project your build will most likely produce a `.jar` file of your code. This code will be validated by your tests. If the whole build/test process passes, then the output of the process (the `.jar`) can be stored as an artifact. The `.jar` file is available to download from our artifacts system long after the workflow that created it has finished.

<img alt="Artifact" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/Diagram-v3-Artifact.png" width="950px" />

```yml
version: 2.1
executors:
  node:
    docker:
      - image: 'circleci/node:8'
    shell: /bin/bash
    working_directory: ~/app
jobs:
  build:
    executor: node
    steps:
      - checkout
      - restore_cache:
          keys:
            # Find a cache corresponding to this specific package-lock.json checksum
            # when this file is changed, this key will fail
            - v1-npm-deps-{{ checksum "package-lock.json" }}
            # Find the most recently generated cache used from any branch
            - v1-npm-deps-
      - run:
          name: Install Node.js dependencies with Npm
          command: npm install
      - save_cache:
          paths:
            - ./node_modules
          key: v1-npm-deps-{{ checksum "package-lock.json" }}
      - persist_to_workspace:
          root: ~/app
          paths:
            - .
  test:
    executor: node
    steps:
      - attach_workspace:
          at: ~/app
      - run:
          name: Test
          command: npm test
      - run:
          name: Save test results
          command: |
            npm install nyc mocha-junit-reporter
            mkdir ~/reports
            ./node_modules/.bin/nyc ./node_modules/.bin/mocha spec.js --recursive --timeout=10000 --exit --reporter mocha-junit-reporter --reporter-options mochaFile=~/reports/mocha/test-results.xml
          environment:
            MOCHA_FILE: ~/reports/test-results.xml
          when: always
      - store_test_results:
          path: ~/reports
      - store_artifacts:
          path: ~/reports
workflows:
  version: 2
  build_and_test:
    jobs:
      - build
      - test:
          requires:
            - build
```