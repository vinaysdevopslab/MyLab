pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage3 : Publish the source code to Sonarqube


        stage ('Publish the sourcecode to sonarqube and SonarQube analysis'){
            steps {
                echo ' publishing the source code to sonarqube......'
                withSonarQubeEnv('sonarqube') { // You can override the credential to be used
                sh 'mvn sonar:sonar'
                }
            }

            }
        
    }

}