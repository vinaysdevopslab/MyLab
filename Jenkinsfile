pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }



    stages {
        // Specify various stage with in stagesx

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testings
        stage ('Test'){
            steps {
                echo ' testing...20k.xdream.'

            }
        }

        // Stage3 : Publish the source code to Sonarqubes
       // stage ('Sonarqube Analysis'){
            //steps {
                //echo ' Source code published to Sonarqube for SCA......'
               // withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                    // sh 'mvn sonar:sonar'
                

            
        

        
        
    }
}






