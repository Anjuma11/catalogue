pipeline {

    agent {
        label "AGENT-1"
    }

    // environment{
    //     COURSE="Jenkins"
    // }
    options{
        timeout(time: 10, unit: "SECONDS")
        disableConcurrentBuilds()
    }

    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    // }

    
    stages {

        stage('Read package.json'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Package version: ${appVersion}"
                    
                }
            }
        }
        stage('Install dependencies'){
            steps{
                script{
                    sh """
                        npm install
                    """
                }
            }
        }

        stage('Build') {
            steps {
                script{
                    sh """
                    echo "building..."
                    env
                    """
                }
                
            }
        }
        stage('Test') {
            steps {
                echo "testing..."
            }
        }
        stage('Deploy') {
            steps {
                echo "deploying..."
            }
        }
    }
    
    post{
        always{
            echo "I will always say hello again"
            deleteDir()
        }
        success{
            echo "Hello Success..."
        }
        failure{
            echo "Hello Failure..."
        }

    }
}