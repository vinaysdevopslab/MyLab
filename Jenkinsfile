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

                script {
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "dev-proj-SNAPSHOT" : "dev-proj"
                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '',
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']],
                credentialsId: '724da07c-0a37-4345-9c29-ef853eda98e6', 
                groupId: "${GroupId}", 
                nexusUrl: '54.236.249.57:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
                }

            }
        }


        
        
    }

}
