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
        stage('Deploy artifacts to Artifactory'){
            steps{
                echo 'This is a post-build actions'
                rtUpload(
                    serverId: 'jfrog-jenkins',
                    spec: """{
                        "files": [
                                {
                                    "pattern": "multibranch-pipeline2/master/build/libs/*.jar",
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
    }
}
