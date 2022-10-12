pipeline { 
    agent any  
    stages { 
        stage('Build') { 
            steps { 
               echo 'This is a minimal pipeline.' 
            }
        }
        stage('post-build') {
            steps {
               echo 'doing post-build actions'
            }
        }
    }
}
