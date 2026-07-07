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
    stage('Build') { 
      steps {
        sh 'python3 -m py_compile sources/add2vals.py sources/calc.py' 
        stash(name: 'compiled-results', includes: 'sources/*.py*') 
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