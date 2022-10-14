pipeline { 
    def server = Artifactory.server 'jfrog-jenkins'
    def uploadSpec = """{
    "files": [{
                "pattern": "multibranch-pipeline2/master/build/libs/*.jar",
                "target": "libs-snapshot-local"
            }
        ]
    }"""
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
                server.upload(uploadSpec)
            }
        }
    }
}
