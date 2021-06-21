pipeline{
    agent any
    tools{
        maven "maven381"
    }
    stages{
        stage('checkout'){
            steps{
                git credentialsId: 'Git_cred', 
                url: 'https://github.com/naveenyp31/praa.git'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Tests'){
            steps{
                sh 'mvn test'
                junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
            }
        }
        stage('deploy'){
            steps{
              sshagent(['tomcat9']) {
                  sh """
                  scp -o StrictHostKeyChecking=no target/*.war ec2-user@10.0.0.239:/opt/apache-tomcat-9.0.46/webapps/
                  ssh ec2-user@10.0.0.239 /opt/apache-tomcat-9.0.46/bin/shutdown.sh
                  ssh ec2-user@10.0.0.239 /opt/apache-tomcat-9.0.46/bin/startup.sh
                  """ 
                } 
            }         
        }
    }
}