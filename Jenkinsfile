
pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "iyedbhd"
        Front_TAG = "${DOCKERHUB_USERNAME}/devops_project:angular"
    }

    stage('Git') {
      steps {
        withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
          echo 'Pulling from Front git'
          git branch: 'main',
            url: "https://${GITHUB_TOKEN}@github.com/RayenMajdoub12/5SIM2-BT-Front.git"
        }
      }
    }

      stage('Clean') {
            steps {
                sh 'rm -rf node_modules'
                sh 'npm cache clean --force'
                sh'npm cache clean -f'
            }
        }

        stage('Install') {
            steps {
              sh 'npm install @popperjs/core --legacy-peer-deps'
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Build') {
            steps {
                
                sh 'ng build --configuration=production --output-path=dist'
              //sh 'ng serve --watch --proxy-config proxy.conf.json'
                echo 'Build stage done'
            }
        }

    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'DOCKER', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
          sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
        }
      }
    }

      stage('Build Docker') {
      steps {
            sh "docker build -t $FRONT_TAG ."
      }
        }
      

     stage('Docker Push') {
      steps {
        sh "docker push ${FRONT_TAG}"
      }
    }

      
}