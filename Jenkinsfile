pipeline {
  agent {
    docker {
      image 'gradle:jdk11'
    }

  }
  stages {
      
    stage('clone') {
      steps {
          checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CloneOption', noTags: false, reference: '', shallow: true]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/lpesola/jenkins-katas/']]]
          stash excludes: '.git', name: 'katacode'
      }
    }
      
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
              unstash 'katacode'
              sh 'ci/build-app.sh'
          }
         options {
              skipDefaultCheckout()
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
