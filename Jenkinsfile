pipeline { 
    agent any 
    tools{
        maven "maven"
    }
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    } 
    stages { 
        stage('Build') { 
            steps { 
                echo 'Build stage' 
                sh 'mvn clean package'
            }
        }
        stage('SonarQube analysis'){
            steps{
                withSonarQubeEnv('SonarQube-8.9.9') { 
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage ('Deploy'){
            steps{
                echo 'Development deployment'
                script {
                    deploy adapters: [tomcat9(credentialsId: 'tomcat-cred', path: '', url: 'http://tomcatserver.centralindia.cloudapp.azure.com:8080/')], contextPath: '', onFailure: false, war: 'webapp/target/*.war' 
                }
            }
        }
    }
}
