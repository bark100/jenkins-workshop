// This is a declarative Jenkins pipeline!

pipeline {
    agent {
        kubernetes {
            defaultContainer 'sbt'
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: sbt
    image: hseeberger/scala-sbt:8u252_1.3.13_2.13.3
    command:
    - cat
    tty: true
    resources:
      requests:
        cpu: "1000m"
        memory: "512Mi"
"""
        }
    }
    stages {
        stage('Compile') {
            steps {
                sh 'sbt compile'
            }
        }
        stage('Unit Tests') {
            steps {
                sh "sbt 'testOnly -- -n UnitTest'"
            }
        }
        stage('Slow tests') {
            when {
                branch 'master'
            }
            steps {
                sh "sbt 'testOnly -- -n Slow'"
            }
        }
    }
    post {
        always {
            junit 'target/junit/**/*.xml'
        }
    }
}
