pipeline {
    agent {
        docker { image 'node:16-alpine' } // Top-level agent, simple and likely cached
    }

    environment {
        REGULAR_VARIABLE = "This is just a regular variable"
    }

    stages {
        stage('Access Secret') {
            steps {
                withCredentials([string(credentialsId: 'my-jenkins-secret', variable: 'MY_SECRET_ENV_VAR')]) {
                    sh 'echo "Attempting to use the Jenkins secret..."'
                    sh 'echo "The regular variable is: $REGULAR_VARIABLE"'
                    sh 'echo "The secret value (should be masked) is: $MY_SECRET_ENV_VAR"'
                    sh 'echo "To confirm the secret was passed, its length is: ${#MY_SECRET_ENV_VAR}"'
                    sh 'echo "Done with secret."'
                }
            }
        }

        stage('Build Artifact') { // <<< --- NEW STAGE STARTS HERE ---
            // This stage will also use the top-level 'node:16-alpine' agent
            steps {
                sh 'echo "Creating a simple build artifact..."'
                sh 'echo "Hello from my build process! This is output.txt" > output.txt'
                sh 'echo "Contents of output.txt:"'
                sh 'cat output.txt'
                sh 'ls -l' // To show the file exists in the workspace

                // Use the archiveArtifacts step to save output.txt
                archiveArtifacts artifacts: 'output.txt', fingerprint: true

                sh 'echo "Artifact should be archived."'
            }
        } // <<< --- NEW STAGE ENDS HERE ---
    }
}
