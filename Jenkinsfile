node{
    stage('Checkout Code'){
        git url: 'https://github.com/dsrdsr90/web_proj_working.git'
    }
    
     stage('Build Code'){
        sh 'mvn clean package'
        echo 'Build Completed'
    }
    
    stage('Artifactory upload'){
        def server = Artifactory.server 'jfrog-server'
        def buildNumber = currentBuild.number
        def uploadSpec = """{
          "files": [
            {
                "pattern": "/home/ubuntu/.jenkins/workspace/${env.JOB_NAME}/webapp/target/*.war",
                "target": "libs-snapshot-local/${buildNumber}/"
            }
         ]
        }"""
        def buildInfo = server.upload spec:uploadSpec
        server.publishBuildInfo buildInfo
    }
}
