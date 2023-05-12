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
        // Specify various stage with in stagesx

        // stage 1. Build
        stage('Build') {
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testings
        stage('Test') {
            steps {
                echo ' testing...20k.xdream.'
            }
        }

        //3 publish the artifacts to nexus

        stage('publish to nexus') {
            steps {
                script {
                    def NexusRepo = Version.endsWith('SNAPSHOT') ? 'tarisdream4k-SNAPSHOT' : 'tarisdream8k-RELEASE'

                    nexusArtifactUploader artifacts:
                [[artifactId: "${ArtifactId}",
                classifier: '',
                file:"target/${ArtifactId}-${Version}.war",
                type: 'war']], credentialsId: 'bd6fac4a-b642-4fe1-bfee-525097abe0f0',
                groupId: "${GroupId}", nexusUrl: '172.20.10.121:8081',
                nexusVersion: 'nexus3',
                protocol: 'http',
                repository: "${NexusRepo}",
                version: "${Version}"
                }
            }
        }

          //STAGE4 PRINT SOME INFORMATION

        stage('print environmental variables') {
            steps {
                echo "artifact id is  '${ArtifactId}'"
                echo "version is '${Version}'"
                echo "group Id is '${Name}'"
                echo "the name is'${GroupId}'"
            }
        }
        // Stage3 : Publish the source code to Sonarqubes
       // stage ('Sonarqube Analysis'){
            //steps {
                //echo ' Source code published to Sonarqube for SCA......'
               // withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                    // sh 'mvn sonar:sonar'

        stage 5 : Deploying

        stage ('Deploy') {
    steps {
        echo "deploying"
        sshPublisher(publishers: [
            sshPublisherDesc(configName: 'ansible-control-node', 
                transfers: [sshTransfer(
                    cleanRemote: false, 
                    excludes: '', 
                    execCommand: '/opt/playbooks/ansible-playbook downloadanddeploy.yml -i hosts', 
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
