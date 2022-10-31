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
        stage ('Deploy'){
            steps{
                echo 'Development deployment'
                script {
                    deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://tomcatserver.centralindia.cloudapp.azure.com:8080/')], contextPath: '', onFailure: false, war: 'webapp/target/*.war' 
                }
            }
        }
    }
}
