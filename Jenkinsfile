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
            //    sh 'mvn -Dmaven.test.failure.ignore=true install'
                dir("/var/lib/jenkins/workspace/multibranch-pipeline2_${branch}")
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Deploy artifacts to Artifactory'){
            steps{
                echo 'This is a post-build actions in dev branch'
                rtUpload(
                    serverId: 'jfrog-jenkins',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "multibranch-pipeline2/master/build/*.jar",
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
        stage ('Deploy to production'){
            when{
                branch 'master'
            }
            steps{
                echo 'Production deployment'
            }
        }
    }
}
