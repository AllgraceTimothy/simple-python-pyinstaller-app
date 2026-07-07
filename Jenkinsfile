pipeline {
  agent {
    docker {
        image 'python:3.12'
    }
  }
  stages {
    stage('Install dependencies') {
      steps {
        sh 'python3 -m pip install -r requirements.txt'
      }
    }
    stage('Build') { 
      steps {
        sh 'python3 -m py_compile sources/add2vals.py sources/calc.py' 
        stash(name: 'compiled-results', includes: 'sources/*.py*') 
      }
    }
    stage('Test') { 
      steps {
        sh 'pytest --junit-xml test-reports/results.xml sources/test_calc.py' 
      }
      post {
        always {
          junit 'test-reports/results.xml' 
        }
      }
    }
  }
}