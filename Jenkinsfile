pipeline {
    agent any  // Use any available agent
    stages {
        stage('Checkout Repositories') {
            steps {
                script {
                    // Checkout the First repository into a different directory
                    dir('argocd') {
                        checkout([
                            $class: 'GitSCM', 
                            branches: [[name: '*/main']],
                            userRemoteConfigs: [[
                                credentialsId: 'git_id',
                                url: 'https://github.com/pkrishnamohan007/argocd.git'
                            ]]
                        ])
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline Succeeded'
        }
        failure {
            echo 'Pipeline Failed'
        }
    }
}
