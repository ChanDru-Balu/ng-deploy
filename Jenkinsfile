pipeline{
    agent any
       tools {
        // Define the Docker tool in the 'tools' section
        // This assumes 'myDockerTool' is configured in Jenkins
        dockerTool 'MyDockerTool'
    }
    environment {
        // Define environment variables if needed
        dockerImage = 'ngdeploy'
    }

    stages{
        stage('Clone Repository'){
            steps{
                // New Git Repo
                git 'https://github.com/ChanDru-Balu/ng-deploy'
            }
        }

        stage('Build Angular Production') {
            steps {
                script {
                    // Navigate to Angular project directory
                    dir('angular-project') {
                        // Build Angular project for production
                        bat "npm install -g @angular/cli"
                        bat "npm install"
                        bat "ng build --output-path docs --base-href /"
                    }
                }
            }
        }

        stage('Build Docker Image'){
            steps{
                script{
                //     sh '/usr/local/bin/docker build -t reusable-image .'

                   // Get the current working directory
                    def currentDir = pwd()

                    // Define the path to the Dockerfile relative to the Jenkinsfile
                    def dockerfilePath = "${currentDir}/Dockerfile"

                    // Build Docker image
                    bat "docker build -t ngdeploy -f ${dockerfilePath} ."
                } 
            }
        }
    
        stage('Run Docker Container'){
            steps{
                script {
                    // sh '/usr/local/bin/docker run -p 8090:80 reusable-image .'

            // Run Docker container
            bat "docker run -p 8090:80 -d --name deployContainer ngdeploy"
                }
            }
        }

    }
}
