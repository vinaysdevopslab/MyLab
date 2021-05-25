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
        // Stage 3 : Deploying
        stage('Deploy'){
            steps {
                echo 'deploying-----------'
            }
        }
    }     
}