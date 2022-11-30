pipeline {
    agent any

    tools {
        maven 'apache-maven-latest'
        jdk 'openjdk-jdk11-latest'
    }

    environment {
        EMAIL_TO = "ndoschek+eclipseci@eclipsesource.com, eneufeld+eclipseci@eclipsesource.com"
    }

    stages {
        stage ('Build Checkstyle project') {
            steps {
               dir('codestyle/org.eclipse.emfcloud.checkstyle') {
                    sh 'mvn clean verify ' 
                }
            }
        }
     
        stage('Deploy') {
            when { 
                allOf {
                    branch 'master';
                    expression {  
                      /* Only trigger the deployment job if files inside the `codestyle/org.eclipse.emfcloud.checkstyle` 
                      are part of the change set.*/ 
                      sh(returnStatus: true, script: 'git diff --name-only HEAD^ | grep --quiet "^codestyle/org.eclipse.emfcloud.checkstyle"') == 0
                    }
                }
            }
            steps {
            	build job: 'deploy-emfcloud-checkstyle-m2', wait: false
            }
        }
    }

    post {
        failure {
            script {
                if (env.BRANCH_NAME == 'master') {
                    echo "Build result FAILURE: Send email notification to ${EMAIL_TO}"
                    emailext attachLog: true,
                    body: 'Job: ${JOB_NAME}<br>Build Number: ${BUILD_NUMBER}<br>Build URL: ${BUILD_URL}',
                    mimeType: 'text/html', subject: 'Build ${JOB_NAME} (#${BUILD_NUMBER}) FAILURE', to: "${EMAIL_TO}"
                }
            }
        }
        unstable {
            script {
                if (env.BRANCH_NAME == 'master') {
                    echo "Build result UNSTABLE: Send email notification to ${EMAIL_TO}"
                    emailext attachLog: true,
                    body: 'Job: ${JOB_NAME}<br>Build Number: ${BUILD_NUMBER}<br>Build URL: ${BUILD_URL}',
                    mimeType: 'text/html', subject: 'Build ${JOB_NAME} (#${BUILD_NUMBER}) UNSTABLE', to: "${EMAIL_TO}"
                }
            }
        }
        fixed {
            script {
                if (env.BRANCH_NAME == 'master') {
                    echo "Build back to normal: Send email notification to ${EMAIL_TO}"
                    emailext attachLog: false,
                    body: 'Job: ${JOB_NAME}<br>Build Number: ${BUILD_NUMBER}<br>Build URL: ${BUILD_URL}',
                    mimeType: 'text/html', subject: 'Build ${JOB_NAME} back to normal (#${BUILD_NUMBER})', to: "${EMAIL_TO}"
                }
            }
        }
    }
}
