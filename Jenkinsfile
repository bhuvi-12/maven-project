pipeline { 
    agent any 
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    } 
    stages { 
        stage('Build') { 
            steps { 
               echo 'This is a minimal pipeline.' 
            }
        }
        stage('SonarQube analysis') {
            steps{
                withSonarQubeEnv('SonarQube-8.9.9') { 
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage('post-build') {
            steps {
               echo 'post-build actions'
            }
        }
    }
}


// pipeline{
//     agent any
//     environment {
//         PATH = "$PATH:/usr/share/maven/bin"
//     }
//     stages{
//        stage('GetCode'){
//             steps{
//                 git 'https://github.com/bhuvi-12/maven-project'
//             }
//          }        
//        stage('Build'){
//             steps{
//                 sh 'mvn clean package'
//             }
//          }
//         stage('SonarQube analysis') {
//             steps{
//                 withSonarQubeEnv('SonarQube-8.9.9') { 
//                     sh "mvn sonar:sonar"
//                 }
//             }
//         }
       
//     }
// }