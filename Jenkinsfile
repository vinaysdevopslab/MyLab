pipeline{
//Directives
    agent any
    tools{
        maven 'maven'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
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
                
                nexusArtifactUploader artifacts:
                [[artifactId: 'VinayDevOpsLab', 
                classifier: '', 
                file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', 
                type: 'war']], 
                credentialsId: 'f22658a8-67b6-4f7d-b03d-38e13456b8fd', 
                groupId: 'com.vinaysdevopslab', 
                nexusUrl: '172.20.10.90:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'VinaysDevOpsLab-SNAPSHOT', 
                version: '0.0.4-SNAPSHOT'
            }
        }
        //Stage 4 : Print some information
        stage ('Print Environment variables'){
                steps {
                    echo "Artifact ID is '${ArtifactId}'"
                    echo "Version is '${Version}'"
                    echo "GroupID is '${}'"
                    echo "Name is '${Name}'"
                }
        }
        // Stage 4 : Deploying
        stage('Deploy'){
            steps {
                echo 'deploying-----------'
            }
        }
    }     
}