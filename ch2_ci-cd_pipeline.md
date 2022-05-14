# CI/CD Pipeline: Learn with Example

## What is a CI/CD pipeline?

A CI/CD pipeline automates the process of software delivery. It builds code, runs tests, and helps you to safely deploy a new version of the software. CI/CD pipeline reduces manual errors,
provides feedback to developers, and allows fast product iterations.

CI/CD pipeline introduces automation and continuous monitoring throughout the lifecycle of a software product. It involves from the integration and testing phase to delivery and deployment. These connected practices are referred as CI/CD pipeline.

## What is Continuous Integration, Continuous Delivery, and Continuous Deployment?

- **Continuous integration** is a software development method where members of the team can integrate their work at least once a day. In this method, every integration is checked by an automated build to search the error.
- **Continuous delivery** is a software engineering method in which a team develops software products in a short cycle. It ensures that software can be easily released at any time.
- **Continuous deployment** is a software engineering process in which product functionalities are delivered using automatic deployment. It helps testers to validate whether the codebase changes are correct, and it is stable or not.

In this CI/CD Pipeline tutorial, you will learn:

- [Stages of a CI-CD pipeline](#stages-of-a-ci-cd-pipeline)
- [Example of CI-CD Pipeline](#example-of-ci-cd-pipeline)
- [CI-CD pipeline Best Practices](#ci-cd-pipeline-best-practices)
- [Advantages of CI-CD pipelines](#advantages-of-ci-cd-pipelines)
- [Important CI-CD tools](#important-ci-cd-tools)
- [Why Does the CI-CD Pipeline Matter for IT Leaders?](#why-does-the-ci-cd-pipeline-matter-for-it-leaders)
- [CI-CD Pipeline KPI](#ci-cd-pipeline-kpi)

## Stages of a CI-CD pipeline

A CI/CD pipeline is a runnable specification of the steps that any developer should perform to
deliver a new version of any software. Failure in each and every stage triggers a notification via email, Slack, or other communication platforms. It enables responsible developers to know about the important issues.

Here are the important Stages of CI/CD pipeline:

<p align="center"><img src="../images/CI-surecleri/image-11.png" alt="Stages of CI/CD pipeline"></p>

<h4 align="center">Stages of CI/CD pipeline</h4>

### Source Stage

In the source stage, CI/CD pipeline is triggered by a code repository. Any change in the program
triggers a notification to the CI/CD tool that runs an equivalent pipeline. Other common triggers include user-initiated workflows, automated schedules, and the results of other pipelines.

### Build Stage

This is the second stage of the CI/CD Pipeline in which you merge the source code and its
dependencies. It is done mainly to build a runnable instance of software that you can potentially ship to the end-user.

Programs that are written in languages like C++, Java, C, or Go language should be compiled. On the other hand, JavaScript, Python, and Ruby programs can work without the build stage.

Failure to pass the build stage means there is a fundamental project misconfiguration, so it is better that you address such issue immediately.

### Test Stage

Test Stage includes the execution of automated tests to validate the correctness of code and the behaviour of the software. This stage prevents easily reproducible bugs from reaching the clients. It is the responsibility of developers to write automated tests.

### Deploy Stage

This is the last stage where your product goes live. Once the build has successfully passed through all the required test scenarios, it is ready to deploy to live server.

## Example of CI-CD Pipeline

Here is example of CI/CD pipeline:

- **Source Code Control:** Host code on GitHub as a private repository. This will help you to integrate your application with major services and software.
- **Continuous integration:** Use continuous integration and delivery platform CircleCI and commit every code. When the changes notify, this tool will pull the code available in GitHub and process to build and run the test.
- **Deploy code to UAT:** Configure CitcleCI to deploy your code to AWS UAT server.
Deploy to production: You have to reuse continuous integration steps for deploying code to
UAT.

## CI-CD pipeline Best Practices

Here is a CI/CD pipeline best practices:

- Write up the current development process therefore, you can know the procedures that require to change and one that can be easily automated.
- Start off with a small proof of project before going ahead and complete whole development process at once.
- Set up a pipeline with more than one stage in which fast fundamental tests run first.
- Start each workflow from the same, clean, and isolated environment.
- Run open source tools that cover everything from code style to security scanning.
- Setup a better code hub to continuously check the quality of your code by running the standard set of tests against every branch.
- Peer code review each pull request to solve a problem in a collaborative manner.
- You have to define success metrics before you start the transition to CD automation. This will help you to consistently analyze your software, developing progress help refining where needed.

## Advantages of CI-CD pipelines

Here are the pros/ benefits of CI/CD Pipeline:

- Builds and testing can be easily performed manually.
- It can improve the consistency and quality of code.
- Improves flexibility and has the ability to ship new functionalities.
- CI/CD pipeline can streamline communication.
- It can automate the process of software delivery.
- Helps you to achieve faster customer feedback.
- CI/CD pipeline helps you to increase your product visibility.
- It enables you to remove manual errors.
- Reduces costs and labour.
- CI/CD pipelines can make the software development lifecycle faster.
- It has automated pipeline deployment.
- A CD pipeline gives a rapid feedback loop starting from developer to client.
- Improves communications between organization employees.
- It enables developers to know which changes in the build can turn to the brokerage and to avoid them in the future.
- The automated tests, along with few manual test runs, help to fix any issues that may arise.

## Important CI-CD tools

Here are the important CI/CD tools:

### Jenkins

<p align="center"><img src="../images/CI-surecleri/image-12.png"></p>

Jenkins is an open-source Continuous Integration server that helps to achieve the Continuous Integration process (and not only) in an automated fashion. Jenkins is free and is entirely written in Java. Jenkins is a widely used application around the world that has around 300k installations and growing day by day.

**Features:**

- Jenkin will build and test code many times during the day.
- Automated build and test process, saving timing, and reducing defects.
- The code is deployed after every successful build and test.
- The development cycle is fast.

Link: <https://www.jenkins.io/download/>

### Bambo

<p align="center"><img src="../images/CI-surecleri/image-13.png"></p>

Bamboo is a continuous integration build server that performs - automatic build, test, and releases in a single place. It works seamlessly with JIRA software and Bitbucket.

**Features:**

- Run parallel batch tests
- Setting up Bamboo is pretty simple
- Per-environment permissions feature allows developers and QA to deploy to their environments
- Built-in Git branching and workflows. It automatically merges the branches.

Link: <https://www.atlassian.com/software/bamboo>

### CircleCI

<p align="center"><img src="../images/CI-surecleri/image-14.png"></p>

Circle CI is a flexible CI tool that runs in any environment like a cross-platform mobile app, Python API server, or Docker cluster. This tool reduces bugs and improves the quality of the application.

**Features:**

- Allows to select Build Environment
- Supports many languages including C++, JavaScript, NET, PHP, Python, and Ruby
- Support for Docker lets you configure a customized environment.
- Automatically cancel any queued or running builds when a newer build is triggered.

Link: <https://circleci.com/>

## Why Does the CI-CD Pipeline Matter for IT Leaders?

- CI/CD pipeline can improve reliability.
- It makes IT team more attractive to developers.
- CI/CD pipeline helps IT leaders, to pull code from version control and execute software build.
- Helps to move code to target computing environment.
- Enables project leaders to easily manage environment variables and configure for the target environment.
- Project managers can publish push application components to services like web services, database services, API services, etc.
- Providing log data and alerts on the delivery state.
- It enables programmers to verify code changes before they move forward, reducing the chances of defects ending up in production.

## CI-CD Pipeline KPI

- **Cycle or Deployment Time:** Cycle time is the time taken to go from the build stage to production. You can obtain average life cycle time by measuring the development process phases. This metric will give insight into bottlenecks in your process and the overall speed of development time.

- **Development Frequency:** Development frequency allows you to analyse bottlenecks you find during automation. The more frequent smaller releases reduce the risk of defects and fix them when found. Such a metric is an overall measure of your team efficiency.

- **Change Lead Time:** It measures the start time of the development phase to deployment. This metric is an indicator of the entire development process and how well the team works together.

- **Change Failure Rate:** It focuses on the number of times development get succeeds vs. the number of times it fails.

- **MTTR vs. MTTF:** MTTR (Mean Time to Recovery) is the amount of time required by your team to recover from failure. MTTF (Mean Time to Failure) measures the amount of time between fixes and outages. These metrics are a reflection of the team's ability to respond and fix issues.

### Summary

- A CI/CD pipeline automates the process of software delivery.
- CI/CD pipeline introduces automation and continuous monitoring throughout the lifecycle of a software product.
- Continuous integration is a software development method where members of the team can integrate their work at least once a day.
- Continuous delivery is a software engineering method in which a team develops software products in a short cycle.
- Continuous deployment is a software engineering process in which product functionalities are delivered using automatic deployment.
- There are four stages of a CI/CD pipeline 1) Source Stage, 2) Build Stage, 3) Test Stage, 4) Deploy Stage.
- Important CI/CD tools are Jenkins, Bambo, and Circle CI.
- CI/CD pipeline can improve reliability.
- CI/CD pipeline makes IT team more attractive to developers.
- Cycle time is the time taken to go from the build stage to production.
- Development frequency allows you to analyse bottlenecks you find during automation.
- Change Lead Time measures the start time of the development phase to deployment.
- Change Failure Rate focuses on the number of times development get succeeds vs. the number of times it fails.
- MTTR (Mean Time to Recovery) is the amount of time required by your team to recover from failure.
- MTTF (Mean Time to Failure) measures the amount of time between fixes and outages.
