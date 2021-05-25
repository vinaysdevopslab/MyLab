pipeline{
//Directives
    agent any
    tools{
        maven 'maven'
    }
    stages {
       // Specify various stage within stages

       // Stage 1. build
        stage ('Build'){
          steps {
              sh 'mvn clean install package'
          }  
        }

        // stage 2 : Testing
        stage ('Test'){
            steps {
                echo 'testing...........'
            }
        }
        
        // Stage 3 :Publish artifact to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/com.vinaysdevopslab-0.0.9.war', type: 'war']], credentialsId: 'f2ef7fe4-c07e-4d53-a963-6048833c2b10', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.90:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'SyedDevOpsLab-SNAPSHOT', version: '0.0.9'
            }
        }
        // Stage 3 : Deploying
        stage('Deploy'){
            steps {
                echo 'deploying-----------'
            }
        }
    }     
}