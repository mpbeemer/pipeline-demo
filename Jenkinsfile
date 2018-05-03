pipeline {
    agent any

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
                    sh "cp -f **/target/*.war /usr/share/tomcat-development"
                    input 'Promote to production?'
                    sh "cp -f **/target/*.war /usr/share/tomcat-production"
                }
                failure {
                    echo 'Unable to deploy - packaging failed.'
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
