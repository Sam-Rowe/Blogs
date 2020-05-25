# Title
Azure DevOps Continuous Integration: To Make or not to make in CI - that is the question

# Teaser
There are many great native tasks for Azure DevOps CI pipelines from running container builds through to Running a Sonar Cloud Analysis. With so much choice you can build the whole CI pipeline with a series of tasks, but is that the right thing to do? Read more for a discussion of use cases and suggested patterns to make the choice easier.

# Post

When building an Azure Pipeline for CI (continuous integration) there are many tasks available in the tool kit to chose from. These have a wide range of uses. Extensions for Azure DevOps can add even more.  

Traditionally developers used build tooling on their local machines such as Make to configure, Compile, test and Clean up source code during the development process.  

Doesn't that sounds an awful lot like a CI pipeline?

## Comparison of the tools  
Secrets and configuration:  
Azure Pipeline tasks are integrated with the Azure Pipeline agent baking secret management and build time configuration into the CI process. This is achieved through the Agent exposing Environment Variables at pipline run time that contain these configured values. The variables can of course be backed by Azure Key Vault for secrets, Variable groups for sharing configuration between multiple pipelines or on the CI pipline itself. These variables can even be set at the point of being queued.  

Make and other build tooling:
. Configuration can be passed with with config files which can be shared and overridden by the developer. 


