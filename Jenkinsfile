pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        sh 'printenv'
        sh 'docker build -t ecr-demoing .'
      }
    }
    stage ('Publish ECR') {
      steps {
        withEnv (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/e4j0o0u9'
          sh 'docker build -t ecr-demoing .'
          sh 'docker tag ecr-demoing:latest public.ecr.aws/e4j0o0u9/ecr-demoing:""$BUILD_ID""'
          sh 'docker push public.ecr.aws/e4j0o0u9/ecr-demoing:""$BUILD_ID""'
        }
      }
    }
  }
}
