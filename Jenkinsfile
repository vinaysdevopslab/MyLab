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
                credentialsId: '6c00c76e-7db1-4ea4-9059-22ef887ff0ec', 
                groupId: "${GroupId}", 
                nexusUrl: '44.206.249.123:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
                }

            }
        }

        // Stahe4: Deploy to tomcat using ansible
        stage('Deploy to Tomcat'){
            steps{
                   sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible_Controller', 
                   transfers: [sshTransfer(cleanRemote: false, excludes: '',
                   execCommand: 'ansible-playbook /opt/playbooks/deploy_to_nexus.yml -i /opt/playbooks/hosts',
                   execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false,
                   patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false,)], 
                   usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])


            }


        }


        
        
    }

}
