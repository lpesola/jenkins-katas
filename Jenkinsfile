pipeline {
  agent {
    docker {
      image 'gradle:jdk11'
    }

  }
  stages {
    stage('Parallel execution') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "pul °°° <(((-<"'
            sh 'echo "<((-<"'
          }
        }

        stage('Build App') {
          steps {
            sh 'ci/build-app.sh'
          }
        }

      }
    }

    stage('archive') {
      steps {
        archiveArtifacts 'app/build/libs/'
      }
    }

  }
}