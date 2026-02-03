pipeline {
	agent any
	
	// 전역변수 => ${SERVER_IP}
	environment {
			SERVER_IP = "aws ip"
			SERVER_USER = "ubuntu"
			APP_DIR = "~/app"
			JAR_NAME = "SpringTotalProject-0.0.1-SNAPSHOT.war"
	}
		
	stages {
		
		 연결 확인 = ngrok
		 stage('Check Git Info') {
			steps {
				sh '''
				    echo "===Git Info==="
				    git branch
				    git log -1
				   '''
			}
		}
		
		// 감지 = main : push (commit)
		stage('Check Out') {
			steps {
				 echo 'Git Checkout'
                 checkout scm
			}
		}
		
		// gradle build => war파일을 다시 생성 
		stage('Gradle Permission') {
			steps {
				sh '''
				    chmod +x gradlew
				   '''
			}
		}
		
		// build 시작 
		stage('Gradle Build') {
			steps {
				sh '''
				    ./gradlew clean build
				   '''
			}
		}
		
		// war파일 전송 = rsync / scp 
		stage('Docker Build') {
			steps {
					sh '''
						docker build -t leesuhyun1/total-app:latest .
					   '''
				}
			}
		}
		// 실행 명령 
		
		stage('Deploy to Minikube') {
			steps {
					sh '''
						kubectl delete deployment total-app || true
						kubectl apply -f ~/k8s/deployment.yaml
					   '''
				}
			}
		}
		
	}
}