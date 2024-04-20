pipeline{
    agent any
    stages{
        stage("Lint"){
            steps{
                sh "pip3 install flake8"
                sh  "python3 -m flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics"
            }
        }
        stage("Unit Tests"){
            steps {
                sh '''
                  pip3 install -r requirements.txt
                  pip3 install --upgrade pytest pytest-cov pytest-html pytest-sugar pytest-json-report
                  python3 -m pytest -v --cov --html=reports/pytest/report.html
                '''
                archiveArtifacts artifacts: 'reports/pytest/*',
                   allowEmptyArchive: true,
                   fingerprint: true,
                   onlyIfSuccessful: true
            }
        }
        stage("Docker"){
            steps{
                sh "docker build . -t otomato/fastapi-example:${currentBuild.number}"
            }
        }
    }
}
