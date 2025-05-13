pipeline {
    agent any
    environment {
        // 定义环境变量
        DEPLOYMENT_NAME = "teedy-deployment"  // 替换为你的K8s部署名称
        CONTAINER_NAME = "teedy-container"    // 替换为你的容器名称
        TEEDY_IMAGE = "sismics/docs:v1.11"    // 使用官方Teedy镜像
    }
    
    stages {
        stage('Start Minikube') {
            steps {
                sh '''
                if ! minikube status | grep -q "Running"; then
                    echo "Starting Minikube..."
                    minikube start --force
                else
                    echo "Minikube already running."
                fi
                '''
            }
        }

        stage('Update Deployment') {
            steps {
                sh """
                # 更新部署使用指定镜像
                kubectl set image deployment/${DEPLOYMENT_NAME} ${CONTAINER_NAME}=${TEEDY_IMAGE}
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                kubectl rollout status deployment/${DEPLOYMENT_NAME}
                kubectl get pods -l app=${DEPLOYMENT_NAME}
                echo "Deployment verification completed!"
                """
            }
        }
    }

    post {
        always {
            echo "Cleaning up..."
            sh 'kubectl get deployments'
        }
    }
}
