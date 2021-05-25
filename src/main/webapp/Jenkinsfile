pipeline{
//Directives
    agent any
    tools{
        maven 'maven'
    }
    Stages {
        // Specify various stage within stages

       // Stage 1. build
       stage ('Build'){
          steps {
              sh 'mvn clean stall package'
          }  
        }
        // stage 2 : Testing
        stage ('Test'){
            steps {
                echo 'testing...........'
            }
        }
        // Stage 3 : Deploying
        Stage('Deploy'){
            steps {
                echo 'deploying-----------'
            }
        }
   }     
