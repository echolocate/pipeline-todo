pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
              // export DATABASE_URI
              sh "export DATABASE_URI"
              // install apt dependencies
              sh "sudo install apt update"
              sh "sudo apt install python3-venv python3-pip python3 -y"
              // create/activate virtual environment
              sh "python3 -m venv venv"
              sh "source venv/bin/activate"
              // install pip requirements (from requirements.txt)
              sh "pip3 install -r requirements.txt"
            }
        }
        stage('Test') {
            steps {
              // run pytest
              sh "python3 -m pytest --cov=application --cov-report term-missing --cov-report xml:coverage.xml --junitxml=junit_report2.xml"
            }
        }
        stage('Deploy') {
            steps {
              // run create.py to create schema
              sh "if [CREATE_SCHEMA == 'true'] then python3 create.py fi"
              // run the app
              sh "python3 app.py"
            }
        }
    }
}
