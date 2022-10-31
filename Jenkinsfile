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
        stage('Deploy artifacts to Artifactory'){
            when{
                branch 'dev'
            }
            steps{
                rtUpload(
                    serverId: 'jfrog-server',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "/var/lib/jenkins/workspace/sample-java-project_dev/server/target/*.jar",
                                    "target": "libs-snapshot-local"
                                }
                        ]
                    }"""
                )
                rtUpload(
                    serverId: 'jfrog-server',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "/var/lib/jenkins/workspace/sample-java-project_dev/webapp/target/*.war",
                                    "target": "libs-snapshot-local"
                                }
                        ]
                    }"""
                )
            }
        }
        // stage ('Publish build info') {
        //     steps {
        //         rtPublishBuildInfo (
        //             serverId: 'jfrog-server'
        //         )
        //     }
        // }
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