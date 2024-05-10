pipeline {
  agent any
  stages {
    stage('SCM checkout') {
      steps {
        echo 'SCM checkout from github'
        git(url: 'https://github.com/Abdomuller11/Dockerize-ECommerce-web-app.git', branch: 'main')
        echo 'repo cloned success in /var/lib/jenkins/workspace/dockercompose'
      }
    }

    stage('make env') {
      steps {
        sh '''cd $WORKSPACE/ecommerce_react_node-main/backend ; echo "MONGO_URI=mongodb+srv://mohamedkhalefaofficial:o2Q4b5GtLxSduDDW@cluster0.mspvlpq.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
JWT_SECRET=sdgkMKEVlm3v23kl_n423vGG3b_TVnm234xnv23
JWT_REFRESH_SECRET=rerv1jv15v1CVBnasd23jnv1j3123nvrqwr23" >  .env'''
      }
    }

    stage('Build') {
      parallel {
        stage('Build backend') {
          steps {
            echo 'i from Build'
            sh 'docker build -t backendfromjenkins --target backend -f Dockerfile .'
          }
        }

        stage('build frontend') {
          steps {
            sh 'docker build -t frontendfromjenkins --target frontend -f Dockerfile .'
          }
        }

        stage('build nginx') {
          steps {
            sh 'docker build -t nginxfromjenkins -f Dockerfile.nginx .'
          }
        }

      }
    }

    stage('Push images to Docker hub') {
      steps {
        echo '[+] pushing now...'
        sh '''docker tag backendfromjenkins abdoemam/ecommerce_app:\'backendfromjenkins\'$BUILD_NUMBER

  '''
        sh '''docker tag frontendfromjenkins abdoemam/ecommerce_app:\'frontendfromjenkins\'$BUILD_NUMBER

'''
      }
    }

    stage('Testing') {
      steps {
        echo 'hi from unit testing'
      }
    }

    stage('Deploy') {
      steps {
        sh 'kubectl apply -f cm.yml ; kubectl apply -f pv.yml ; kubectl apply -f pvc.yml ;   kubectl apply -f deployredis.yml ; kubectl apply -f deploy.yml   ; kubectl apply -f deploynginx.yml ; kubectl apply -f service.yml ; kubectl apply -f ingress.yml'
        sh 'kubectl get all'
        echo 'wow run successfully'
      }
    }

    stage('stop Deploy') {
      steps {
        input(message: 'Do you want me to stop cluster', ok: 'yeeas')
        sh 'kubectl delete all --all -n default'
      }
    }

  }
}