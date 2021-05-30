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
        GroupId = readMavenPom().getGroupId()
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
        
        // Stage3: Publish the source code for SonarQube
        stage ('Sonarqube Analysis'){
            steps {
                echo ' Sorce code published to Sonarqube for SCA...........'
                withSonarQubeEnv('sonarqube') { // You can override the credential to be used
                     sh 'mvn sonar:sonar'
                }
            }
        }
  //  }
//}   
        // Stage 4 :Publish artifact to Nexus 
        stage ('Publish to Nexus'){
            steps {
                script {     
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"
                   
                nexusArtifactUploader artifacts:
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: 'f22658a8-67b6-4f7d-b03d-38e13456b8fd', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.90:8081', 
 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
                
              }   
            }
        }
        //Stage 5 : Print some information
        stage ('Print Environment variables'){
                steps {
                    echo "Artifact ID is '${ArtifactId}'"
                    echo "Version is '${Version}'"
 
                    echo "GroupID is '${GroupId}'"
                    echo "Name is '${Name}'"
                }
        }
        // Stage 6 : Deploying build Artifact to Tomcat 
        stage('Deploy to Tomcat'){
            steps {
                echo 'deploying-----------'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                configName: 'Ansible_Controller', 
                transfers: [
                    sshTransfer(
                             cleanRemote:false,
                             execCommand: 'ansible-playbook  /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                             execTimeout: 120000


                    )
                ],             
                    
                    
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, 
                verbose: false)
                ])
            }
        }       

        // Stage 7 : Deploying build Artifact to Docker
        stage('Deploy to Docker'){
            steps {
                echo 'deploying-----------'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                configName: 'Ansible_Controller', 
                transfers: [
                    sshTransfer(
                             cleanRemote:false,
                             execCommand: 'ansible-playbook  /opt/playbooks/downloadanddeploy_docker.yaml_old -i /opt/playbooks/hosts',
                             execTimeout: 120000


                    )
                ],            
                    
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, 
                verbose: false)
                ])

            }
            
        }
    }     
}