pipeline{
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('Check and remove container'){
            steps{
                script{
                    def containerExists = sh(script: "docker ps -q -f name=webapp", returnStdout: true).trim()
                    if (containerExists) {
                    sh "docker stop webapp"
                    sh "docker rm webapp"
                    }
                }
            }
        }
        stage('Build package'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Build image'){
            steps{
                sh 'sudo docker build -t app /var/lib/jenkins/workspace/web/'
            }
        }
        stage('tag'){
            steps{
                sh 'docker tag app maheshgowdamg25/app'
            }
        }
        stage('Push to dockerhub'){
            steps{
                sh 'echo "Mahi@2001" | docker login -u "maheshgowdamg25" --password-stdin'
                sh 'docker push maheshgowdamg25/app'
            }
        }
        stage('Remove images'){
            steps{
                sh 'docker rmi -f $(docker images -q)'
            }
        }
        stage('Pull image from DockerHub'){
            steps{
                sh 'docker pull maheshgowdamg25/app'
            }
        }
        stage('Run a container'){
            steps{
                sh 'docker run -it -d --name webapp -p 8081:8080 maheshgowdamg25/app'
            }
        }
    }
    post {
        always{
            echo 'Deployed'
        }
        success {
            echo 'successful'
        }
        failure {
            sh 'docker rm -f webapp'
        }
        
    }

}
