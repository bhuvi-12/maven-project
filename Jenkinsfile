pipeline { 
    agent any 
    tools{
        maven "maven"
        jdk "JDK"
    }
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    } 
    stages { 
        stage('SonarQube analysis') {
            steps{
                withSonarQubeEnv('SonarQube-8.9.9') { 
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage('Build') { 
            steps { 
                echo 'Build stage' 
                sh 'mvn clean package'
            }
        }
        stage('Deploy artifacts to Artifactory of DEV branch'){
            when{
                branch 'dev'
            }
            steps{
                rtUpload(
                    serverId: 'jfrog-jenkins',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "/var/lib/jenkins/workspace/multibranch-pipeline2_dev/server/target/*.jar",
                                    "target": "libs-snapshot-local"
                                }
                        ]
                    }"""
                )
                rtUpload(
                    serverId: 'jfrog-jenkins',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "/var/lib/jenkins/workspace/multibranch-pipeline2_dev/webapp/target/*.war",
                                    "target": "libs-snapshot-local"
                                }
                        ]
                    }"""
                )
            }
        }
        stage('Deploy artifacts to Artifactory of Master branch'){
            when{
                branch 'master'
            }
            steps{
                rtUpload(
                    serverId: 'jfrog-jenkins',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "/var/lib/jenkins/workspace/multibranch-pipeline2_master/server/target/*.jar",
                                    "target": "libs-snapshot-local"
                                }
                        ]
                    }"""
                )
                rtUpload(
                    serverId: 'jfrog-jenkins',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "/var/lib/jenkins/workspace/multibranch-pipeline2_master/webapp/target/*.war",
                                    "target": "libs-snapshot-local"
                                }
                        ]
                    }"""
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: 'jfrog-jenkins'
                )
            }
        }

        stage ('Set output resources') {
            steps {
                jfPipelines(
                    outputResources: """[
                        {
                            "name": "pipelinesBuildInfo",
                            "content": {
                                "buildName": "${env.JOB_NAME}",
                                "buildNumber": "${env.BUILD_NUMBER}"
                            }
                        }
                    ]"""
                )
            }
        }
        stage ('Deploy'){
            when{
                branch 'dev'
            }
            steps{
                echo 'Development deployment'
                script {
                    deploy adapters: [tomcat9(credentialsId: 'tomcat-new', path: '', url: 'http://apachetomcatserver.westus3.cloudapp.azure.com:8080/')], contextPath: '', onFailure: false, war: 'webapp/target/*.war' 
                }
            }
        }
    }
}
