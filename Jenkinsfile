pipeline {
    agent any

    tools {
        maven 'apache-maven-latest'
        jdk 'openjdk-jdk11-latest'
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
}
