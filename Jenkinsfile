pipeline {
    agent {
        label "AGENT-1"
    }

    environment {
        COURSE = "jenkins"
    }
    options {
        timeout(time=30,units="MINUTES")
        disableConcurrentBuilds()
    }
    stages {
        stage("Build") {
            steps {
                sh """
                    echo "hello world"
                """
            }
        }
        stage ("Test") {
            steps {
                echo "Testing..."
            }
        }
        stage ("Deploy") {
            steps {
                echo "Deploying"
            }
        }
    }
    post {
        always {
            echo "hello from always"
            deleteDir()
        }
        success {
            echo "hello from success"
        }
        failure {
            echo "hello from failure"
        }
    }
}