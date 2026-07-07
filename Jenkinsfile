pipeline {
  agent {
    docker {
        image 'python:3.12'
    }
  }
  options {
    skipStagesAfterUnstable()
  }
  stages {
    stage('Install dependencies') {
      steps {
        sh '''
          python3 -m venv venv
          . venv/bin/activate
          pip install --upgrade pip
          pip install -r requirements.txt
        '''
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
        . venv/bin/activate
        sh 'pytest --junit-xml test-reports/results.xml sources/test_calc.py' 
      }
      post {
        always {
          junit 'test-reports/results.xml' 
        }
      }
    }
    stage('Deliver') { 
      steps {
        sh "pyinstaller --onefile sources/add2vals.py" 
      }
      post {
        success {
          archiveArtifacts 'dist/add2vals' 
        }
      }
    }
  }
}