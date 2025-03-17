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
                echo "🔎 Checking for pom.xml file..."
                if [ -f "pom.xml" ]; then
                    echo "✅ pom.xml found. Starting Maven build..."
                    mvn clean install
                else
                    echo "❌ pom.xml not found. Skipping build."
                    exit 1
                fi
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo "🔎 Running Maven tests..."
                mvn test
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "🔎 Checking for Dockerfile..."
                if [ -f "Dockerfile" ]; then
                    echo "✅ Dockerfile found. Building Docker image..."
                    docker build -t $DOCKER_IMAGE .
                else
                    echo "❌ Dockerfile not found. Skipping Docker build."
                    exit 1
                fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "🚀 Deploying Docker container..."
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
