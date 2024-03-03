pipeline{
    agent{
        label "docker"
    }
    stages{
        stage("Lint"){
            steps{
                sh "pip3 install flake8"
                sh  "flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics"
            }
        }
        stage("Unit Tests"){
            steps {
                sh '''
                  pip install -r requirements.txt
                  pip install --upgrade pip pytest pytest-cov pytest-html pytest-sugar pytest-json-report
                  py.test -v --cov --html=reports/pytest/report.html
                '''
                archiveArtifacts artifacts: 'reports/pytest/*',
                   allowEmptyArchive: true,
                   fingerprint: true,
                   onlyIfSuccessful: true
            }
        }
        stage("Docker"){
            steps{
                sh "docker build otomato/fastapi-example:${currentBuild.number}"
            }
        }
    }
}
