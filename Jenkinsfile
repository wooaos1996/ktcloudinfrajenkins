pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/wooas1996/ktcloudinfrajenkins.git', branch: 'main'
      }
    }
    stage('docker image build and push to hub') {
      steps {
        sh '''
        docker build -t kwongeonwoo/ktcloudinfra4:0727 .
        docker push kwongeonwoo/ktcloudinfra4:0727
        '''
      }
    }
    stage('delivery and deployment using k8s') {
      steps {
        sh '''
        ansible master -m copy -a "src=deploy.yml dest=/root/deploy.yml"
        ansible master -m shell -a "kubectl --kubeconfig=/etc/kubernetes/admin.conf apply -f deploy.yml"
        '''
      }
    }
  }
  post {
      success {
          echo 'Pipeline succeeded!'
          // 성공에 따른 스크립트 동작등도 가능하다!!!  결과 코드 : 0
      }
      failure {
          echo 'Pipeline failed!'
      }
  }
}
