pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    #stage('Detect project1 changes') {
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
            // stop pipeline gracefully
            return
          } else {
            echo "project1 changed → continue"
          }
        }
      }
    }

    stage('Build (training)') {
      steps {
        echo "Run simple build steps for training"
        // example step
        bat 'type project1\\README.md || echo project1 exists'
      }
    }

    stage('Publish') {
      steps {
        echo "Publish / archive (optional for training)"
      }
    }
  } // pipeline stages
  post {
    always { echo "Pipeline finished with status: ${currentBuild.currentResult}" }
  }
}
