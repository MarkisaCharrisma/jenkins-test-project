pipeline {
    agent {
        docker { image 'node:16-alpine' } // Using a simple, quick-to-pull image
    }

    environment {
        REGULAR_VARIABLE = "This is just a regular variable"
    }

    stages {
        stage('Access Secret') {
            steps {
                withCredentials([string(credentialsId: 'my-jenkins-secret', variable: 'MY_SECRET_ENV_VAR')]) {
                    // The Jenkins credential with ID 'my-jenkins-secret' 
                    // is now available as an environment variable named MY_SECRET_ENV_VAR
                    // within this block.

                    sh 'echo "Attempting to use the Jenkins secret..."'
                    sh 'echo "The regular variable is: $REGULAR_VARIABLE"'
                    sh 'echo "The secret value (should be masked) is: $MY_SECRET_ENV_VAR"'
                    sh 'echo "To confirm the secret was passed, its length is: ${#MY_SECRET_ENV_VAR}"'
                    sh 'echo "Done with secret."'
                }
                // MY_SECRET_ENV_VAR is no longer available outside the withCredentials block
            }
        }
    }
}
