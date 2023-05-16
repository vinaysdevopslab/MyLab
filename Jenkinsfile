pipeline {
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version    = readMavenPom().getVersion()
        Name       = readMavenPom().getName()
        GroupId     = readMavenPom().getGroupId()
    }

    stages {
        // Specify various stages

        // Stage 1. Build
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        // Stage2 : Testing
        stage('Test') {
            steps {
                echo ' testing...20k.xdream.'
            }
        }

        //Stage3 : Publish the source code to Sonarqube
        stage ('Sonarqube Analysis'){
            steps {
                echo ' Source code published to Sonarqube for SCA......'
                withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                     sh 'mvn sonar:sonar'
                }

            }
        }


        // Stage3: Publish the artifacts to nexus
        stage('publish to nexus') {
            steps {
                script {
                    def NexusRepo = Version.endsWith('SNAPSHOT') ? 'tarisdream4k-SNAPSHOT' : 'tarisdream8k-RELEASE'

                    nexusArtifactUploader artifacts:
                    [[artifactId: "${ArtifactId}",
                    classifier: '',
                    file:"target/${ArtifactId}-${Version}.war",
                    type: 'war']], credentialsId: 'bd6fac4a-b642-4fe1-bfee-525097abe0f0',
                    groupId: "${GroupId}", nexusUrl: '18.208.131.219:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: "${NexusRepo}",
                    version: "${Version}"
                }
            }
        }

        // Stage4: Print environmental variables
        stage('print environmental variables') {
            steps {
                echo "artifact id is  '${ArtifactId}'"
                echo "version is '${Version}'"
                echo "group Id is '${GroupId}'"
                echo "the name is '${Name}'"
            }
        }

        // Stage5: Deploying
        stage('Deploy') {
            steps {
                echo 'deploying'
                sshPublisher(publishers: [
                    sshPublisherDesc(configName: 'ansible-control-node',
                        transfers: [sshTransfer(
                            cleanRemote: false,
                            excludes: '',
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yml -i /opt/playbooks/hosts',
                            execTimeout: 120000,
                            flatten: false,
                            makeEmptyDirs: false,
                            noDefaultExcludes: false,
                            patternSeparator: '[, ]+',
                            remoteDirectory: '',
                            remoteDirectorySDF: false,
                            removePrefix: '',
                            sourceFiles: ''
                        )]
                    , usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false)
                ])
            }
        }

        // Stage6: Deploying the build artifacts to docker
        stage('Deploy to docker') {
            steps {
                echo 'deploying'
                sshPublisher(publishers: [
                    sshPublisherDesc(configName: 'ansible-control-node',
                        transfers: [sshTransfer(
                            cleanRemote: false,
                            excludes: '',
                            /* groovylint-disable-next-line LineLength */
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy-docker-NEW-STYLE.yml -i /opt/playbooks/hosts',
                            execTimeout: 190000,
                            flatten: false,
                            makeEmptyDirs: false,
                            noDefaultExcludes: false,
                            patternSeparator: '[, ]+',
                            remoteDirectory: '',
                            remoteDirectorySDF: false,
                            removePrefix: '',
                            sourceFiles: ''
                        )]
                    , usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false)
                ])
            }
        }
    }
}
