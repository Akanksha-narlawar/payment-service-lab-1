pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Secret Removed') {
            steps {
                powershell '''
                    git grep "DEBUG_API_KEY" $(git rev-list --all)

                    if ($LASTEXITCODE -eq 1) {
                        Write-Host "PASS: DEBUG_API_KEY not found in Git history."
                        exit 0
                    }
                    elseif ($LASTEXITCODE -eq 0) {
                        Write-Host "FAIL: DEBUG_API_KEY still exists in Git history."
                        exit 1
                    }
                    else {
                        Write-Host "Git verification command failed."
                        exit 1
                    }
                '''
            }
        }

        stage('Git Status') {
            steps {
                powershell 'git status'
            }
        }

        stage('Git History') {
            steps {
                powershell 'git log --oneline --graph --all'
            }
        }
    }
}