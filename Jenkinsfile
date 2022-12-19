node {
    agent any
    stage('Build'){
        git url: 'https://github.com/dsrdsr90/web_proj_working.git'
        withMaven(
            maven: 'maven',

        ){
            sh 'mvn clean package'
        }
    }
}