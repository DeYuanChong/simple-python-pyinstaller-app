
pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    environment {
        VENV_PATH = 'C:\\Users\\dylan\\OneDrive\\Desktop\\Jenkins\\.venv\\Scripts\\activate' // Set this to your virtual environment path
    }
    stages {
        stage('Build') {
            steps {
                bat '''
                call %VENV_PATH%
                python -m py_compile sources/add2vals.py sources/calc.py
                '''
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') {
            steps {
                bat '''
                call %VENV_PATH%
                pytest --junit-xml test-reports/results.xml sources/test_calc.py
                '''
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                bat '''
                call %VENV_PATH%
                pyinstaller --onefile sources/add2vals.py
                '''
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals.exe' 
                }
            }
        }
    }
}