pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    parameters {
         string (name: 'tomcat_dev', defaultValue: '52.56.89.11', description: 'Staging Server')
         string (name: 'tomcat_prod', defaultValue: '52.56.251.150', description: 'Production Server')
    }

    triggers {
         pollSCM ('* * * * *')
     }

    stages{
        stage ('Build'){
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /my-keys/jenkinsdemo.pem **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /my-keys/jenkinsdemo.pem **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}

