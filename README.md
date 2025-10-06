# Build a CI/CD Pipeline with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codepipeline-updated)

**Author:** Łukasz Brodziak  
**Email:** lukasz.brodziak@gmail.com

---

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Introducing Today's Project!

In this project, I will prepare CI/CD pipeline automation using CodePipeline. Thanks to this I will combine the, now separated, tools into a true CI/CD with no need for manual steps.

### Key tools and concepts

Services I used was CodePipeline Key concepts I learnt include creating, configuring and running a pipeline.

### Project reflection

This project took me approximately 30. It was most rewarding to see the pipeline to be triggered after a change in the code has been made.

---

## Starting a CI/CD Pipeline

AWS CodePipeline is a service that helps with building an automated CI/CD pipeline. Using CodePipeline makes sure your deployments are consistent, reliable and happen automatically whenever we update our code - with less risk of human errors! It saves us time too.

CodePipeline offers different execution modes based on way we want to have multiple pipeline runs handled. I chose superseded in which if a new pipeline is run while other pipelines are running the newer execution will immediately take over and cancel the older one. This is perfect for making sure only the latest code changes are processed, which is exactly what we want for our CI/CD pipeline!  Other options include: Queued mode, where executions are processed one after another. If a pipeline is already running, any new executions will wait in a queue until the current execution finishes. Parallel mode allows multiple executions to run at the same time, completely independently of each other. This can speed up the overall processing time if you have multiple branches or code changes that can be built and deployed concurrently.

A service role gets created automatically during setup so that CodePipline is able to perform all needed actions automatically without user intervention.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_gdnhtm)

---

## CI/CD Stages

The three stages I've set up in my CI/CD pipeline are Source, Build and Deploy. 

CodePipeline organizes the three stages into a single diagram that shocases the flow from Source to Deploy. In each stage, you can see more details on the pipeline and services each step connects to or runs.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Source Stage

In the Source stage, the default branch tells CodePipeline to monitor the specified branch for any code changes commited to it.

The source stage is also where you enable webhook events, which let CodePipeline automatically start your pipeline whenever code is pushed to our specified branch in GitHub. This is what makes our pipeline truly "continuous" – it reacts to code changes in real-time! 
How do Webhooks work?
Webhooks are like digital notifications. When you enable webhook events, CodePipeline sets up a webhook in your GitHub repository. This webhook is configured to listen for specific events, such as code pushes to the master branch.

Whenever you push code to the master branch, GitHub sends a webhook event (a notification) to CodePipeline. CodePipeline then automatically starts a new pipeline execution in response to this event. It's a seamless way to automate your CI/CD process!

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_sergt)

---

## Build Stage

The Build stage sets up a compilation and packaging of the application. The input artifact for the build stage is the output from the previous stage that are used as inputs for the current stage. In our Build stage, we're using SourceArtifact, which is the ZIP file containing our source code that was outputted by the Source stage.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_j1k2l3m4)

---

## Deploy Stage

The Deploy stage is the final step in our pipeline. It's responsible for taking the application artifacts (the output from the Build stage) and deploying them to the target environment, which in our case is an EC2 instance. I configured the Deploy provider which was set to CodeDeploy, next I configured the CodeDeploy application and deployment group.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_m4n5o6p7)

---

## Success!

Since my CI/CD pipeline gets triggered bya change in code I tested my pipeline by replacing existing text on the web page with <p>If you see this line, that means your latest changes are automatically deployed into production by CodePipeline!</p>


The moment I pushed the code change CodePipeline triggered a new execution of the pipeline. The commit message under each step indicates that it was really triggerd by a chenge in code.

Once my pipeline executed successfully, I checked the website by goingo to the public IPv4 DNS address I got from the Deploy stage of the pipeline.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_e1f2g3h4)

---

## Testing the Pipeline

In a project extension, I initiated a rollback on Deploy stage. Automatic rollback is important for keeping the application running without errors introduced in a commit.

During the rollback, the source and build stage are unaffected by the rollback.

After rollback, the live web app reverted to the version from last successful deployment, and the line added to trigger the deployment was gone.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-devops-codepipeline-updated_sdfgsdfgdf)

---
## Wrap up
This project wraps up the a 7 day series of creating building blocks of the CI/CD pipeline in AWS.

