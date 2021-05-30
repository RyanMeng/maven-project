pipeline {
    agent any

    tools{
        maven 'local_maven'
    }

    parameters{
        string(name: 'tomcat_dev', defaultValue: '18.234.169.199', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '54.157.132.1', description: 'Production Server')
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
                    echo '开始存档...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

     stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/ryanmeng/.ssh/tomcat-demo **/target/*.war ec2-user@${params.tomcat_dev}:/usr/local/apache-tomcat-9.0.46/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/ryanmeng/.ssh/tomcat-demo **/target/*.war ec2-user@${params.tomcat_prod}:/usr/local/apache-tomcat-9.0.46/webapps"
                    }
                }
            }
        }
    }
}