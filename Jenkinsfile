pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Secret Removed From Git History') {
            steps {
                powershell '''
                    git grep "DEBUG_API_KEY" $(git rev-list --all -- config/application.yml) -- config/application.yml

                    if ($LASTEXITCODE -eq 1) {
                        Write-Host "PASS: DEBUG_API_KEY not found in config/application.yml Git history."
                        exit 0
                    }
                    elseif ($LASTEXITCODE -eq 0) {
                        Write-Host "FAIL: DEBUG_API_KEY still exists in config/application.yml Git history."
                        exit 1
                    }
                    else {
                        Write-Host "Git history verification failed."
                        exit 1
                    }
                '''
            }
        }

        stage('Git Status') {
            steps {
                powershell '''
                    git status
                '''
            }
        }

        stage('Git History') {
            steps {
                powershell '''
                    git log --oneline --graph --all
                '''
            }
        }
    }
}