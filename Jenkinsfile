pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="994940421254.dkr.ecr.ap-south-1.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'project'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws configure set aws_access_key_id AKIA6PJYSASDAJ7MHHFU
         aws configure set aws_secret_access_key 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
         aws configure set default.region ap-south-1
         #echo $AWS_ACCESS_KEY_ID
         #DOCKER_LOGIN_PASS=$(aws ecr get-login-password  --region us-east-1
         #docker login -u AWS -p $DOCKER_LOGIN_PASS https://994940421254.dkr.ecr.ap-south-1.amazonaws.com/project1
         aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 994940421254.dkr.ecr.ap-south-1.amazonaws.com
	 docker build -t 994940421254.dkr.ecr.ap-south-1.amazonaws.com/project1:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push 994940421254.dkr.ecr.ap-south-1.amazonaws.com/project1:SAMPLE-PROJECT-${BUILD_NUMBER}
	  '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
            sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
            kubectl apply -f deployment-new.yaml -n $K8S_NAMESPACE
            kubectl apply -f service.yaml -n $K8S_NAMESPACE
           '''
      }
    }
  }
}
