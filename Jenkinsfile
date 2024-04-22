pipeline{
    agent any
    environment {
        TO_EMAIL_ADDRESS = credentials('main_dest_email')
    }
    stages{
        stage("Lint"){
            steps{
                sh "pip3 install flake8 --break-system-packages"
                sh  "python3 -m flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics"
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
        stage("Docker build"){
            steps{
                sh "docker build . -t otomato/fastapi-example:${currentBuild.number}"
            }
        }
        stage("Docker Run"){
            steps{
                sh "docker run --name fast_api -d otomato/fastapi-example:${currentBuild.number}"
                sh "echo 'Container will run for 3 minutes'"
                sh "sleep 180"
                sh "docker rm -f fast_api"
            }
        }
    }
    post {
        always {
            emailext body: "Build ${currentBuild.number} completed, log in to see detailed information.", subject: "Fastapi build process notification.", to: env.TO_EMAIL_ADDRESS
        }
    }
}
