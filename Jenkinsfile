pipeline {
  agent {
    docker {
      image 'gradle:jdk11'
    }
  }
  environment {
    HUB_USR = 'lpesola'
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

        stage('test app') {
          steps {
              unstash 'katacode'
              sh 'ci/unit-test-app.sh'
              junit 'app/build/test-results/test/TEST-*.xml'
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

    stage('docker push') {
      environment {
        DOCKERCREDS = credentials('docker_hub_login')
      }
      steps {
        unstash 'katacode'
        sh 'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin && echo "Logged in to hub for $HUB_USR"'
        sh 'ci/push-docker.sh'
      }
    }

    stage('archive') {
      steps {
        archiveArtifacts 'app/build/libs/'
      }
    }

  }
  post {
    always {
      deleteDir() 
    }
  }
}
