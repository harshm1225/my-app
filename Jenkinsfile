pipeline {
    agent none  // Specify that the pipeline does not use a global agent
    environment {
        NETLIFY_SITE_ID = '111a1412-1e93-47bb-9a18-e1ff0a216af0'
    }
    stages {
        stage('Build') {
            // Define the agent specific to this stage using Docker
            agent {
                docker {
                    // Use the 'node:20.16.0-slim' Docker image for this stage
                    image 'node:20.16.0-bookworm-slim'
                    // Reuse the same Docker container for subsequent steps within this stage
                    reuseNode true
                }
            }
            steps {
                // Install Node.js dependencies
                sh 'npm ci'
                
                // Build the project using the installed dependencies
                sh 'npm run build'

                sh 'ls -la'
            }
        }
        
        stage('Test'){
          // Define the agent specific to this stage using Docker
            agent {
                docker {
                    // Use the '20.16.0-bookworm-slim' Docker image for this stage
                    image 'node:20.16.0-bookworm-slim'
                    // Reuse the same Docker container for subsequent steps within this stage
                    reuseNode true
                }
            }
            steps {
              sh 'npm run test'
              // Check build folder for index.html
              sh 'test -f build/index.html'
            }
        }

        stage("Deploy"){
          agent {
                docker {
                    // Use the 'node:20.16.0-slim' Docker image for this stage
                    image 'node:20.16.0-bookworm-slim'
                    // Reuse the same Docker container for subsequent steps within this stage
                    reuseNode true
                }
            }
         steps {

         sh '''
            npm install netlify-ci
            node_modules/.bin/netlify --version
         ''' 
         }
        }
    }
}
