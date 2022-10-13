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
               sh './mvnw clean install'
            }
        }
        stage('SonarQube analysis') {
            steps{
                withSonarQubeEnv('SonarQube-8.9.9') { 
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage('Deploy artifacts to Artifactory'){
            agent{
                docker{
                    image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0'
                    reuseNode true
                }
            }
            steps{
                echo 'This is a post-build actions'
                sh 'jfrog rt upload --url http://jfrog.centralindia.cloudapp.azure.com:8081/artifactory/ target/demo-0.0.1-SNAPSHOT.jar libs-snapshot-local'
            }
        }
    }
}
