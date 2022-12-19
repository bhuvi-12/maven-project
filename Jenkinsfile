node {
    agent any

    stage('Build'){
        git url: 'https://github.com/bhuvi-12/maven-project.git'
        withMaven(
            maven: 'maven',
            
        ){
            sh 'mvn clean package'
        }
    }
}