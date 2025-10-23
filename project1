pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                bat """
                @echo off
                echo === Project 1: Hello Jenkins ===
                echo Hello from Project 1!
                dir
                """
            }
        }
    }
}
