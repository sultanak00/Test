pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'python3 -m py_compile add2vals.py calc.py'
                stash(name: 'compiled-results', includes: '*.py*')
            }
        }
        stage('Test') { 
            steps {
                
                sh 'python3 -m pytest --junit-xml test-reports/results.xml test_calc.py' 
            }
            post {
                always {
                    junit 'test-reports/results.xml' 
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh 'brew  install numpy pyinstaller'
                sh "pyinstaller --onefile add2vals.py" 
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals' 
                }
            }
        }
    }
}
