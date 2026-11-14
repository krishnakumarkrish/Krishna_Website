pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Pulls code from your GitHub repository using your credentials
                git branch: 'main', credentialsId: 'Cjenkins', url: 'https://github.com/krishnakumarkrish/Krishna_Website.git'
            }
        }
		
		stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('SonarQube-Server') {
                    // Explicitly bind the credential ID 'sonar-qube' to SONAR_TOKEN
                     withCredentials([string(credentialsId: 'sonar-qube', variable: 'SONAR_TOKEN')]) {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.token=${env.SONAR_TOKEN}"
                        }
                    }
                }        
            }
        }
		

        stage('Stop Old Container') {
            steps {
                // Safely stops and removes the old container if it exists, ignoring errors if it doesn't
                sh 'docker stop portfolio_krishna_container || true'
                sh 'docker rm portfolio_krishna_container || true'
            }
        }

        stage('Clear old Docker Image') {
            steps {
                // Removes the old docker image
                sh 'docker rmi portfolio_krishna || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Builds the updated Docker image and tags it as portfolio:latest
                sh 'docker build --no-cache -t portfolio_krishna:latest .'
            }
        }

        stage('Run New Container') {
            steps {
                // Runs the new container on port 8090 mapped to port 80 inside the container
                sh 'docker run -d --name portfolio_krishna_container -p 8091:80 portfolio_krishna:latest'
            }
        }
    }
}
