pipeline {
    agent any

    tools {
        maven 'apache-maven-latest'
        jdk 'openjdk-jdk11-latest'
    }

    stages {
        stage ('Build Checkstyle project') {
            steps{
               dir('codestyle/org.eclipse.emfcloud.checkstyle'){
                    sh 'mvn clean verify ' 
                }
            }
        }
     
        stage('Deploy') {
            when { 
                allOf {
                    branch 'master';
                    changeset  pattern: "codestyle/org.eclipse.emfcloud.checkstyle", comparator: "REGEXP";  
                }
            }
            steps {
            	build job: 'deploy-emfcloud-checkstyle-m2', wait: false
            }
        }
    }
}
