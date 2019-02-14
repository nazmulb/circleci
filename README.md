# Circle CI

## What is CI/CD? 

**Continuous Integration (CI)** - is the practice of merging all developer working copies to a shared mainline several times a day. A complementary practice to CI is that before submitting work, each programmer must do a complete build and run (and pass) all unit tests. Integration tests are usually run automatically on a CI server (Jenkins/Drone/CercleCI) when it detects a new commit.

**Continuous Delivery (CD)** - It aims at building, testing, and releasing software with greater speed and frequency. Delivery team -> Version Control -> Build & unit tests -> Automated acceptance tests (e2e) -> UAT -> Release (Deployment, DevOps) 

<img alt="CI/CD Process" src="https://raw.githubusercontent.com/nazmulb/circleci/master/images/cicd.png" width="850px" />

## What is CircleCI?

CircleCI is Continuous Integration, a development practice which is being used by software teams allowing them to to build, test and deploy applications easier and quicker on multiple platforms.

CircleCI uses a <a href="https://en.wikipedia.org/wiki/YAML">YAML</a> (`.circleci/config.yml`) configuration file:

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