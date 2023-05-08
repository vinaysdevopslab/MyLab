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

        //3 publish the artifacts to nexus

        stage('publish to nexus') {
             steps {
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.4.war', type: 'war']], credentialsId: 'bd6fac4a-b642-4fe1-bfee-525097abe0f0', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.121:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'tarisdream4k-SNAPSHOT', version: ' 0.0.4'
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






