pipeline {
  agent any
  stages {
    stage('Source') {
      steps {
        git 'https://github.com/AnjaneyareddyTetala/sam-hello-world.git'
      }
    }

    stage('Version') {
      steps {
        sh 'sam --version'
      }
    }

    stage('Validate') {
      steps {
        dir(path: 'sam-hello-world-1') {
          sh 'sam validate'
          catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            sh 'exit 0'
          }

        }

      }
    }

    stage('build') {
      steps {
        dir(path: 'sam-hello-world-1') {
          sh 'cp -avr /home/package.json . '
          sh 'sam build'
        }

      }
    }

    stage('package') {
      steps {
        dir(path: 'sam-hello-world-1') {
          sh 'sam package --output-template-file sam-template-mumbai.yaml --s3-bucket sambucketjenkinspractice'
        }

      }
    }

    stage('deploy') {
      input {
        message 'Do you want to proceed for production deployment?'
      }
      steps {
        dir(path: 'sam-hello-world-1') {
          sh 'sam deploy --template-file sam-template-mumbai.yaml --stack-name sambucketjenkins --capabilities CAPABILITY_IAM'
        }

      }
    }

  }
}
