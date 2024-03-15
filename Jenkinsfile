pipeline {
    agent any

    tools {
        nodejs "nodejs 17.4.0"
    }

    environment {
        SNYK_TOKEN = credentials('snyk_token')
    }
    stages {
        stage('Download Latest Snyk CLI') {
            steps {
                script {

                    snyk_cli_dl_linux="https://github.com/snyk/snyk/releases/download/v1.838.0/snyk-linux"
                    echo "Download URL: ${snyk_cli_dl_linux}"

                    sh """
                        curl -Lo ./snyk "${snyk_cli_dl_linux}"
                        chmod +x ./snyk
                        mkdir -p reports
                        ls -la
                        ./snyk -v
                        ./snyk auth $SNYK_TOKEN
                    """
                }
            }
        }
        stage('Get npm and snyk-to-html'){
            steps {
                script {
                    sh """
                        npm install
                        npm install snyk-to-html -g
                    """
                }
            }
        }
        stage('Test Snyk') {
            steps {
                script {
                    sh "./snyk test --json | snyk-to-html -o reports/results.html"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    
    }
}
