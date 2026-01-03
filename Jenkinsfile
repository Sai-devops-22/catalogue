pipeline {
    agent {
        label "AGENT-1"
    }

    environment {
        COURSE = "jenkins"
        appVersion = ""
    }
    options {
        timeout(time: 30, unit: "MINUTES")          
        disableConcurrentBuilds()       
    }
    stages {
        stage("Build") {
            steps {
                script{
                    sh """
                        echo "hello world"
                    """
                }

            }
        }
        stage ("Test") {
            steps {
                echo "Testing..."
            }
        }
        stage ("Reading Json file") {
            steps {
                script {
                    def packageJson = readJSON file:"package.json"
                    appVersion = packageJson.version
                    echo "${appVersion}"
                }
            }
        }
        stage("installing dependencies") {
            steps {
                sh """
                    npm install
                """
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



