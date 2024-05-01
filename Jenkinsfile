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

        // stage('Build Docker Image'){
        //     steps{
        //         script{
        //         //     sh '/usr/local/bin/docker build -t reusable-image .'

        //            // Get the current working directory
        //             def currentDir = pwd()

        //             // Define the path to the Dockerfile relative to the Jenkinsfile
        //             def dockerfilePath = "${currentDir}/Dockerfile"

        //             // Build Docker image
        //             bat "docker build -t ngdeploy -f ${dockerfilePath} ."
        //         } 
        //     }
        // }
    
        // stage('Run Docker Container'){
        //     steps{
        //         script {
        //             // sh '/usr/local/bin/docker run -p 8090:80 reusable-image .'

        //     // Run Docker container
        //     bat "docker run -p 8090:80 -d --name deployContainer ngdeploy"
        //         }
        //     }
        // }

        stage('Deploy to GitHub Pages') {
    steps {
        script {

                        // Verify the existence of the docs directory
            def docsDir = 'docs'
            if (!fileExists(docsDir)) {
                error "The 'docs' directory does not exist or is empty."
            }   

            // Copy the build artifacts to a separate directory
            // dir('angular-project') {
                bat "xcopy /s /e /y /i .\\docs gh-pages"
            // }
            
            // Navigate to the gh-pages directory
            dir('../gh-pages') {
                // Check if the gh-pages branch exists
                if (fileExists('.git')) {
                    bat "git status"
                    // If the gh-pages branch exists, checkout the branch
                    bat "git checkout -b gh-pages"
                } else {
                    // If the gh-pages branch doesn't exist, create a new branch
                    bat "git init"
                    bat "git checkout -b gh-pages"
                }
                
                // Copy the contents of the docs directory to the gh-pages branch
                // bat "xcopy /s docs ."
                
                // Add, commit, and push the changes to the gh-pages branch
                bat "git add ."
                bat 'git commit -m "Update GitHub Pages"'
                bat "git push gh-pages"
            }
        }
    }
}


    }
}
