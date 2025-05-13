pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub_credentials')
        DOCKER_IMAGE = 'sismics/docs:v1.11'  // 修改镜像地址
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        // 新增必要变量
        DEPLOYMENT_NAME = 'your-deployment'  // 需替换实际部署名称
        CONTAINER_NAME = 'your-container'    // 需替换实际容器名称
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
                # 添加上下文配置
                minikube update-context >/dev/null 2>&1
                '''
            }
        }

        stage('Set Image') {
            steps {
                sh """
                # 修复变量引用并添加重试
                for i in {1..3}; do
                    echo "Attempt \$i: Setting image for deployment..."
                    kubectl set image deployment/${DEPLOYMENT_NAME} ${CONTAINER_NAME}=${DOCKER_IMAGE} && break
                    sleep 15
                done
                """
            }
        }

        stage('Verify') {
            steps {
                sh '''
                # 添加超时参数
                kubectl rollout status deployment/${DEPLOYMENT_NAME} --timeout=120s
                kubectl get pods
                '''
            }
        }
    }
}
