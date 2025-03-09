pipeline {
    agent any

    stages {
        stage('Generate Flask Dockerfile') {
            steps {
                script {
                    def flaskDockerfile = ''' 
                    FROM python:3.12-slim
                    WORKDIR /app
                    
                    # Install dependencies
                    COPY requirements.txt .
                    RUN pip install --no-cache-dir -r requirements.txt
                    
                    # Copy application code
                    COPY . .

                    # Expose the Flask port
                    EXPOSE 5000

                    # Run the Flask app
                    CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
                    '''
                    writeFile file: 'Dockerfile', text: flaskDockerfile
                }
            }
        }

        stage('Generate Docker Compose File') {
            steps {
                script {
                    def dockerComposeFile = ''' 
                    version: "3.8"
                    services:
                      flask_app:
                        build: .
                        container_name: flask_app
                        ports:
                          - "5000:5000"
                        volumes:
                          - .:/app
                        environment:
                          - FLASK_ENV=development
                    '''
                    writeFile file: 'docker-compose.yml', text: dockerComposeFile
                }
            }
        }

        stage('Build & Run Containers') {
            steps {
                sh 'docker-compose up -d --build'
            }
        }
    }
}
