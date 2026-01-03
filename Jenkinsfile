pipeline {
    agent {
        label "AGENT-1"
    }

    environment {
        COURSE = "jenkins"
        appVersion = ""
        region = "us-east-1"
        acc_id = "911893329385"
        project = "roboshop"
        component = "catalogue"
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
        stage("Docker BUild") {
            steps {
                withAWS(credentials: 'your-jenkins-credential-id', region: 'us-east-1') {
                    sh """
                        aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${acc_id}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${acc_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion} .
                        docker push ${acc_id}.dkr.ecr.${region}.amazonaws.com/${project}/${component}:${appVersion}
                    """
                }
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



