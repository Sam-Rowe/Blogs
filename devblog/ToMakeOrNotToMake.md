# Title
Azure DevOps Continuous Integration: To Make or not to make in CI - that is the question

# Teaser
There are many great native tasks for Azure DevOps CI pipelines from running container builds through to Running a Sonar Cloud Analysis. With so much choice you can build the whole CI pipeline with a series of tasks, but is that the right thing to do? Read more for a discussion of use cases and suggested patterns to make the choice easier.

# Post

When building an Azure Pipeline for CI (continuous integration) there are many tasks available in the tool kit to chose from. These have a wide range of uses. Extensions for Azure DevOps can add even more.  

Traditionally developers used build tooling on their local machines such as Make to configure, Compile, test and Clean up source code during the development process. 

Doesn't that sound an awful lot of overlap with a CI pipeline?

Below we compare the build tools as a group with Azure Pipline tasks then look for the best patterns to make use of the tooling available. First what counts as a build tool for this article? I am thinking not just the venerable [Make](https://www.gnu.org/software/make/), [Rake](https://github.com/ruby/rake), [ANT](http://ant.apache.org/), [Gradle](https://gradle.org/) but also automation tooling in general such as [Maven](https://maven.apache.org/) and [Gulp](https://gulpjs.com/). 


## Comparison of the tools  
## Secrets and configuration  
* CI tooling:  

Azure Pipeline tasks are integrated with the Azure Pipeline agent baking secret management and build time configuration into the CI process. This is achieved through the Agent exposing Environment Variables at pipline run time that contain these configured values. The variables can of course be backed by Azure Key Vault for secrets, Variable groups for sharing configuration between multiple pipelines or on the CI pipline itself. These variables can even be set at the point of being queued.  

* Make and other build tooling:  

Configuration can be passed with with config files which can be shared and overridden by the developer. Secrets for the developer are completely different and tangental to the way a CI pipeline in a DevOps organisation would work. A developer needs access to the whole dev stack to quickly change and experiment to develop the applciation. This must be translatable though to production.

## Building & Testing code
* CI tooling:  
<img width=150px align="right" src="https://raw.githubusercontent.com/Sam-Rowe/Blogs/master/devblog/MavenBuildTaskExample.png">
There are many options here in Azure Pipelines, a command line task to call the complier or specific tasks for the appropriate language. 
These specific tasks have been crafted to maximise the releationship between Azure Pipelines and the langauge specitic tools you are using. 
A great example of this is Maven task which has options to automagically expose the test results to the pipeline, run code ananlysis tools such as SonarCloud and code coverage tools such as JaCoCo by simply enabling them. 

Often however Azure Pipelines, no matter which task is used is basically calling a Make Tool with a task wrapper.

* Make and other build tooling:  

Make tools vary in their inbuilt ability to integrate with external services such as Azure Pipelines or Nexus IQ which makes sense as you expect many developers to one CI. However local Make tools have the ultimate flexibility to go wherever the developers creativity takes them. I recently used Make to coordinate the build and commissioning of an AKS cluster with a group of applications from one repo. Was Make a tool written 40 years ago designed to do that, no, but it is a testiment to its flexibility that I can. 

Unit testing without an automation tool would be tedious. 

## Standardisation
* CI tooling:
Standardized because it runs on the same system each time. You can almost guaruntee that it will build the same output if two developers commit the same source code from different computers. 

This can be achieved by building inside a Docker container though. 

* Make and other build tooling:
Automation is great but every developer will have their own changes that make their environment special. 

Docker is one way to solve this, but consistent CI is also a great tool to enforce the consistency of builds by ensuring they are built on the same machine.


# Make Patterns

* Automation will be run on Dev machine and in CI
* Shift left testing, if it can be done at development time automate it on the Dev build 

# Pipeline pattern


* Integration with outside services
* Fan-in, you don't want every developer poluting a code smell tool with duplicate issues. Fan in and only allow the CI to create the issues and mange the duplications. 
** Integration with Reporting
** Integration with Auditing
* No dev machine in the loop ( such as [Code Spaces](https://github.com/features/codespaces)! or [Power Apps](https://powerapps.microsoft.com/en-us/) ) 
