pipeline {
    agent {
        label "AGENT-1"
    }

    environment {
        COURSE = "jenkins"
        appVersion = ""
        REGION = "us-east-1"
        ACC_ID = "911893329385"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 30, unit: "MINUTES")          
        disableConcurrentBuilds()
        ansiColo()       
    }
    parameters {
        booleanParam(name: "deploy", defaultValue: false, description:"CHECK")
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
                withAWS(credentials: 'aws-creds', REGION: 'us-east-1') {
                    sh """
                        aws ecr get-login-password --REGION ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                    """
                }
            }     
        }
        stage("Trigger Build") {
            steps {
                when {
                  expression {params.deploy}  
                }
                script {
                    build job: "catalogue-cd"
                    parameters: [
                        string(name: "appVersion",defaultValue: "${appVersion}"),
                        string(name: "deploy_to", defaultValue: "dev")
                    ],
                    propagate: false
                    wait: false
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



