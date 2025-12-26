pipeline {

    agent {
        label "AGENT-1"
    }

    environment{
        appVersion=""
        region='us-east-1'
        account="759713897143"
        project="roboshop"
        component="catalogue"
    }
    options{
        timeout(time: 30, unit: "MINUTES")
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
        stage('Unit Testing'){
            steps{
                script{
                    sh """
                        echo 'Unit tests'
                    """
                }
            }
        }

        stage('Build docker image'){
            steps{
                script{
                    withAWS(region:'us-east-1', credentials: 'aws-creds') {            
                        sh """
                            aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${account}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${account}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion} .
                            docker push ${account}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                        """
                    }
                }

            }
        }
        // stage('ECR Vulnerability Scan Check') {
        //     steps {
        //         script {
        //             // Fetch scan results
        //             def scanResult = sh(
        //                 script: """
        //                 aws ecr describe-image-scan-findings \
        //                     --repository-name ${repoName} \
        //                     --image-id imageTag=${appVersion} \
        //                     --region ${region} \
        //                     --query 'imageScanFindings.findingSeverityCounts' \
        //                     --output json
        //                 """,
        //                 returnStdout: true
        //             ).trim()

        //             echo "ECR Scan Result: ${scanResult}"

        //             def findings = readJSON text: scanResult

        //             int critical = findings.CRITICAL ?: 0
        //             int high = findings.HIGH ?: 0

        //             if (critical > 0 || high > 0) {
        //                 error("❌ ECR Scan failed: CRITICAL=${critical}, HIGH=${high}")
        //             } else {
        //                 echo "✅ ECR Scan passed. No HIGH or CRITICAL vulnerabilities."
        //             }
        //         }
        //     }
        // }
        // stage('Trigger Deploy'){
        //     when {
        //         expression {params.deploy_to}
        //     }
        //     steps{
        //         script{
        //             build job:"catalogue-cd"
        //                 parameters:[
        //                     string(name:'appVersion', value:"${appVersion}"),
        //                     string(name: 'deploy_to', value:'dev')
        //                 ]
        //                 propagate: "false"
        //                 wait: "false"
        //         }
        //     }
        // }
                
   
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