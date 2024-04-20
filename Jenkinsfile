pipeline{
    agent any
    stages{
        stage("Lint"){
            steps{
                sh "pip3 install flake8 --break-system-packages"
                sh  "python3 -m flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics"
                sh "echo test"
            }
        }
        stage("Unit Tests"){
            steps {
                sh '''
                  pip3 install -r requirements.txt --break-system-packages
                  pip3 install --upgrade pytest pytest-cov pytest-html pytest-sugar pytest-json-report --break-system-packages
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
    post {
        always {
            emailext body: 'Build completed.', subject: 'Fastapi build process...', to: 'misaelramirezf@outlook.com'
        }
    }
}
