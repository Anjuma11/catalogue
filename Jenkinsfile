pipeline {

    agent {
        label "AGENT-1"
    }

    environment{
        COURSE="Jenkins"
    }
    options{
        timeout(time: 10, unit: "SECONDS")
        disableConcurrentBuilds()
    }

    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    }


    stages {
        stage('Build') {
            steps {
                script{
                    sh """
                    echo "building..."
                    echo "Hello ${params.PERSON}"
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