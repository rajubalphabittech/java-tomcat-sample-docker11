pipeline {
    agent any
    stages {
        stage('Build Application') {
            steps {
                sh '/opt/maven/bin/mvn -f pom.xml clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts...."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Create Tomcat Docker Image'){
            steps {
                sh "pwd"
                sh "ls -a"
                /*sh "sudo docker rm -f `sudo docker ps -aq`"*/
                /*sh "sudo docker rm -f '\$(sudo docker ps -aq)'"*/
                sh 'sudo docker ps -aq | sudo docker rm -f'
                sh "sudo docker rmi tomcatsamplewebapp"
                sh "sudo docker images"
                sh "sudo docker build -t tomcatsamplewebapp ."
                sh "sudo docker build . -t tomcatsamplewebapp"
                /*sh "sudo docker build . -t tomcatsamplewebapp:${env.BUILD_ID}"*/
            }
        }

        stage('Deployment'){
            steps {
                sh "sudo docker run -itd -p 8081:8080 tomcatsamplewebapp"
            }
        }

    }
}
