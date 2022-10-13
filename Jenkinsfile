pipeline { 
    agent any 
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
        CI = true
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
    }
    post{
        success{
            steps{
                echo 'This is a post-build actions'
                sh 'jfrog rt upload --url http://jfrog.centralindia.cloudapp.azure.com:8081/artifactory/ target/demo-0.0.1-SNAPSHOT.jar libs-snapshot-local'
            }
        }
    }
}
