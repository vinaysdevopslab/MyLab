pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
       GroupId = readMavenPom().getGroupId()
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
        // Stage3 : Testing
        stage ('Publish to Nexus Repo'){
            steps {
                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '',
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']],
                credentialsId: '724da07c-0a37-4345-9c29-ef853eda98e6', 
                groupId: "${GroupId}", 
                nexusUrl: '3.87.152.174:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'dev-proj-3-snapshot', 
                version: "${Version}"
                

            }
        }


        
        
    }

}
