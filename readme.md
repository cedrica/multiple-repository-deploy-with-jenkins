# This example project view one way to deploy one artifact into two different artifactories.
- different URL
- different Credentials

# Relevant Files are:
## pom.xml
- look at properties part
- look at distributionManagement part
- look at profiles part

## Jenkinsfile
- look at environment section how to access credentials configured into jenkins
- look at "stage('Deploy to second Repository')" how to execute only for specific branch
- look at "stage('Deploy to second Repository')" how to use profile in mvn command
- look at "stage('Deploy to second Repository')" how to use additional parameters for mvn command and refer to Jenkinsfile variables

# Whats not inside of this project?

- Jenkins Credentials configuration
- Artifactory configuration
- Maven settings.xml Server configuration
