pipeline {
    agent any
    
    tools {
        maven 'maven'  // Must match Global Tool config name
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/devopsbyraham/jenkins-java-project.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('S3 Upload') {
            steps {
                s3Upload file:'target/*.war',
                        bucket: 'artifactbucketfornetflixapp',
                        path: 'netflix/',
                        region: 'ap-south-1',
                        credentialsId: 'raham'
            }
        }
        stage('Deploy') {
            steps {
                echo "Deployed to S3: artifactbucketfornetflixapp/netflix/"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
