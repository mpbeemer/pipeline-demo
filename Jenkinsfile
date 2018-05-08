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
                    sh "scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -f **/target/*.war root@172.18.0.4:/usr/local/tomcat/webapps"
                    input 'Promote to production?'
                    sh "scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -f **/target/*.war root@172.18.0.2:/usr/local/tomcat/webapps"
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
