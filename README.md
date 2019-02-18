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
Here `run` is a step type. The `name` attribute is used by the UI for display purposes. The `command` attribute is specific for `run` step and defines command to execute. 

It’s possible to specify a multi-line `command`, each line of which will be run in the same shell:

```yml
- run:
    name: Running tests
    command: |
      echo Running test
      mkdir -p /tmp/test-results
      make test
```

For <a href="https://circleci.com/docs/2.0/configuration-reference/#steps">more info</a>.

#### Using Pre and Post Steps:

Every job invocation may optionally accept two special arguments: `pre-steps` and `post-steps`. Steps under `pre-steps` are executed before any of the other steps in the job. The steps under `post-steps` are executed after all of the other steps.

```yml
version: 2.1
jobs:
  bar:
    machine: true
    steps:
      - checkout
      - run:
          command: echo "building"
      - run:
          command: echo "testing"
workflows:
  build:
    jobs:
      - bar:
          pre-steps:
            - run:
                command: echo "install custom dependency"
          post-steps:
            - run:
                command: echo "upload artifact to s3"
```

#### Conditional Steps:

Conditional steps allow the definition of steps that only run if a condition is met. If `condition` is met, the subkey `steps` are run. A `condition` is a single value that evaluates to `true` or `false` at the time the config is processed.

```yml
version: 2.1
jobs:
  myjob:
    parameters:
      preinstall-foo:
        type: boolean
        default: false
    machine: true
    steps:
      - run: echo "preinstall is << parameters.preinstall-foo >>"
      - when:
          condition: << parameters.preinstall-foo >>
          steps:
            - run: echo "preinstall"

workflows:
  workflow:
    jobs:
      - myjob:
          preinstall-foo: false
      - myjob:
          preinstall-foo: true
```

For <a href="https://circleci.com/docs/2.0/reusing-config/#defining-conditional-steps">more info</a>

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

Workflows define a list of jobs and their run order. It is possible to run jobs in parallel, sequentially, on a schedule, or with a manual gate using an approval job. You can learn more from <a href="https://circleci.com/docs/2.0/workflows/">here</a>.

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

#### Restoring Cache:

CircleCI restores caches in the order of keys listed in the `restore_cache` step. Each cache key is namespaced to the project, and retrieval is prefix-matched. The cache will be restored from the first matching key. If there are multiple matches, the most recently generated cache will be used.

Language dependency manager lockfiles (for example, `package-lock.json`) checksums may be a useful cache key.

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

##### Clearing Cache:

If you need to get clean caches when your language or dependency management tool versions change, use a naming strategy similar to the previous example and then change the cache key names in your `config.yml` file and commit the change to clear the cache.

Caches are immutable so it is useful to start all your cache keys with a version prefix, for example `v1-...`. This enables you to regenerate all of your caches by incrementing the version in this prefix. 

For example, you may want to clear the cache in the following scenarios by incrementing the cache key name:

- Dependency manager version change, for example, you change npm from 4 to 5
- Language version change, for example, you change ruby 2.3 to 2.4
- Dependencies are removed from your project

#### Saving Cache:

To save a cache of a file or directory, add the `save_cache` step to a job with a `key` and `paths` which are required to set. The path for directories is relative to the `working_directory` of your job. You can specify an absolute path if you choose. The caches created via the `save_cache` step are stored for up to **30 days**. For <a href="https://circleci.com/docs/2.0/caching/">more info</a>.

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

You can learn more from <a herf="https://circleci.com/docs/2.0/configuration-reference/#persist_to_workspace">here</a>.

### Artifacts:

Artifacts persist data after a workflow is completed and may be used for longer-term storage of the outputs of your build process. For instance if you have a Java project your build will most likely produce a `.jar` file of your code. This code will be validated by your tests. If the whole build/test process passes, then the output of the process (the `.jar`) can be stored as an artifact. The `.jar` file is available to download from our artifacts system long after the workflow that created it has finished.

<img alt="Artifact" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/Diagram-v3-Artifact.png" width="950px" />

If a job produces persistent artifacts such as screenshots, coverage reports, core files, or deployment tarballs, CircleCI can automatically save and link them for you.

<img alt="Artifacts" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/artifacts.png" width="950px" />

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

For <a href="https://circleci.com/docs/2.0/artifacts/">more info</a>.

### Orbs:

Orbs are packages of config that you can use to quickly get started with the CircleCI platform. Orbs enable you to share, standardize, and simplify config across your projects. Refer to the <a href="https://circleci.com/orbs/registry/">CircleCI Orbs Registry</a> for the complete list of certified orbs.

If your project was added to CircleCI prior to 2.1, you must enable <a href="https://circleci.com/docs/2.0/build-processing/">Build Processing</a> to use the `orbs` key.

```yml
version: 2.1
orbs:
  slack: circleci/slack@volatile
jobs:
  build:
    docker:
      - image: circleci/node:7.10
    steps:
      - slack/notify:
          message: Build of ${CIRCLE_BRANCH} started.
          color: '#42e2f4'
          webhook: https://hooks.slack.com/services/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
      - checkout
      - run: echo "A first hello"
```

You can <a href="https://circleci.com/docs/2.0/using-orbs/">read more</a>.

### Executors:

Executors define the environment in which the steps of a job will be run. When you declare a `job` in CircleCI configuration, you define the type of environment (e.g. `docker`, `machine`, `macos`, etc.) to run in, in addition to any other parameters of that environment, such as:

- environment variables to populate
- which shell to use
- what size `resource_class` to use

When you declare an executor in a configuration outside of `jobs`, you can use these declarations for all jobs in the scope of that declaration, enabling you to reuse a single executor definition across multiple jobs.

An executor definition has the following keys available:

- `docker`, `machine`, or `macos`
- `environment`
- `working_directory`
- `shell`
- `resource_class`

The example below shows an example of using an executor:

```yml
version: 2.1
executors:
  python:
    docker:
      - image: python:3.7.0
      - image: rabbitmq:3.6-management-alpine
    environment:
      ENV: ci
      TESTS: all
    shell: /bin/bash    
    working_directory: ~/project

jobs:
  build:
    executor: python
    steps:
      - checkout
      - run: echo "hello world"

  test:
    executor: python
    environment:
      TESTS: unit
    steps:
      - checkout
      - run: echo "how are you?"

workflows:
  version: 2
  build_test:
    jobs:
      - build
      - test
```

For <a href="https://circleci.com/docs/2.0/reusing-config/#authoring-reusable-executors">more info</a> 

### Reusable Commands:

A reusable command may have the following immediate children keys as a map:

- **Description:** (optional) A string that describes the purpose of the command, used for generating documentation.
- **Parameters:** (optional) A map of parameter keys, each of which adheres to the `parameter` spec.
- **Steps:** (required) A sequence of steps run inside the calling job of the command.

Commands are declared under the `commands` key of a `config.yml` file.

```yml
version: 2.1
commands:
  sayhello:
    description: "A very simple command for demonstration purposes"
    parameters:
      to:
        type: string
        default: "Hello World"
    steps:
      - run: echo Hello << parameters.to >>

jobs:
  myjob:
    docker:
      - image: "circleci/node:9.6.1"
    steps:
      - checkout
      - sayhello:
          to: "Nazmul"
```

### Parameters:

Parameters are declared by name under a job, command, or executor. The immediate children of the `parameters` key are a set of keys in a map.

A parameter can have the following keys as immediate children:

- **Description:** (optional) Used to generate documentation for your orb.
- **type:** (required) See Parameter Types in the section below for details.
- **default:** (optional) The default value for the parameter. If not present, the parameter is implied to be required.

The parameter types supported are:

- string
- boolean
- integer
- enum
- executor
- steps
- environment variable name

The following example defines a command called `sync`:

```yml
version: 2.1
commands: # a reusable command with parameters
  sync:
    parameters:
      from:
        type: string
      to:
        type: string
      overwrite:
        default: false
        type: boolean
    steps:
      - run: # a parameterized run step
          name: Deploy to S3
          command: "aws s3 sync << parameters.from >> << parameters.to >><<# parameters.overwrite >> --delete<</ parameters.overwrite >>"
executors: # a reusable executor
  aws:
    docker:
      - image: cibuilds/aws:1.15
jobs: # a job that invokes the `aws` executor and the `sync` command
  deploy2s3:
    executor: aws
    steps:
      - sync:
          from: .
          to: "s3://mybucket_uri"
          overwrite: true
```

For <a href="https://circleci.com/docs/2.0/reusing-config/#using-the-parameters-declaration">more info</a>

## Using the CircleCI Local CLI:

The CircleCI CLI is a command line interface that leverages many of CircleCI’s advanced and powerful tools from the comfort of your terminal. Some of the things you can do with the CircleCI CLI include:

- Debug and validate your CI config
- Run jobs locally
- Query CircleCI’s API
- Create, publish, view and manage Orbs

### Quick Installation:

#### Mac and Linux:

For the majority of installations, the following commands will get you up and running with the CircleCI CLI:

```
curl -fLSs https://circle.ci/cli | bash
```

#### Install With Homebrew:

If you’re using <a href="https://brew.sh/">Homebrew</a> with macOS, you can install the CLI with the following command:

```
brew install circleci
```

### Configuring The CLI:

Before using the CLI you need to generate a CircleCI API Token from the <a href="https://circleci.com/account/api">Personal API Token tab</a>. After you get your token, configure the CLI by running:

```
circleci setup
```

Setup will prompt you for configuration settings. If you are using the CLI with circleci.com, use the default CircleCI Host.

### Validate A CircleCI Config:

You can avoid pushing additional commits to test your `config.yml` by using the CLI to validate your config locally.

To validate your config, navigate to a directory with a `.circleci/config.yml` file and run:

```
circleci config validate
```

For <a href="https://circleci.com/docs/2.0/local-cli/#section=configuration">more info</a>

## Debugging with SSH:

You can read from <a href="https://circleci.com/docs/2.0/ssh-access-jobs/">here</a>