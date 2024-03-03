pipeline{
    agent{
        label "docker"
    }
    stages{
        stage "Pylint"{
            pip install flake8
            flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
        }
    }
}
