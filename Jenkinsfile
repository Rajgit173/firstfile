pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'myapp-image'
        DOCKER_CONTAINER = 'myapp-container'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Rajgit173/firstfile.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                pwd
                ls -la
                if [ -f "pom.xml" ]; then
                    mvn clean install
                else
                    echo "pom.xml not found. Skipping build."
                    exit 1
                fi
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                if [ -f "pom.xml" ]; then
                    mvn test
                else
                    echo "No tests found. Skipping test stage."
                fi
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                if [ -f "Dockerfile" ]; then
                    docker build -t $DOCKER_IMAGE .
                else
                    echo "Dockerfile not found. Skipping Docker build."
                    exit 1
                fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop $DOCKER_CONTAINER || true
                docker rm $DOCKER_CONTAINER || true
                docker run -d --name $DOCKER_CONTAINER -p 8080:8080 $DOCKER_IMAGE
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}


        
