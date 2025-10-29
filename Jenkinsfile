pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Detect project1 changes') {
      steps {
        script {
          boolean project1Changed = false
          for (changeLog in currentBuild.changeSets) {
            for (entry in changeLog.items) {
              for (file in entry.affectedFiles) {
                echo "Changed: ${file.path}"
                if (file.path ==~ /.*project1.*/) {
                  project1Changed = true
                }
              }
            }
          }

          if (!project1Changed) {
            echo "No changes under 'project1' — skipping build."
            currentBuild.result = 'NOT_BUILT'
            return
          } else {
            echo "project1 changed → continue"
          }
        }
      }
    }

    stage('Build (training)') {
      steps {
        script {
          // safer: list directory contents and only type a file if it exists
          if (fileExists('project1/README.md')) {
            if (isUnix()) {
              sh 'ls -la project1 || true; cat project1/README.md'
            } else {
              bat 'dir project1 || echo dir failed'
              bat 'if exist project1\\README.md ( type project1\\README.md ) else ( echo "project1\\README.md not found" )'
            }
          } else {
            echo "project1/README.md not found — printing project1 contents instead"
            if (isUnix()) {
              sh 'ls -la project1 || true'
            } else {
              bat 'dir project1 || echo project1 folder not found'
            }
          }
        }
      }
    }

    stage('Publish') {
      steps { echo "Publish stage (optional)" }
    }
  }

  post {
    always { echo "Pipeline finished — status: ${currentBuild.currentResult}" }
  }
}
