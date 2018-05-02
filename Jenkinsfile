pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Code Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle findbugs:findbugs pmd:pmd'
            }
        }
        stage('Unit Test'){
            steps {
                sh 'mvn test'
            }
        }

        stage ('Deploy'){
            steps {
                sh 'mvn package'
            }
            post {
                success {
                    sh "cp -f **/target/*.war /home/mpbeemer/projects/tomcat-staging/webapps"
                    input 'Promote to production?'
                    sh "cp -f **/target/*.war /home/mpbeemer/projects/tomcat-prod/webapps"
                }
                failure {
                    echo 'Unable to deply - packaging failed.'
                }
            }
        }
        stage('Integrations Test'){
            steps {
                sh 'mvn verify'
            }
        }
    }
}
