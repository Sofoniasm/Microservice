pipeline {
    agent any

    stages {
        stage('Prepare Namespace') {
            steps {
                sh '''
                    # Create namespace if it doesn't exist
                    if ! kubectl get ns webapps >/dev/null 2>&1; then
                        echo "Creating webapps namespace..."
                        kubectl create ns webapps
                    else
                        echo "Namespace webapps already exists"
                    fi
                '''
            }
        }

        stage('Deploy to LKE') {
            steps {
                sh '''
                    echo "===== Applying Manifests ====="
                    kubectl apply -f deployment-service.yml -n webapps
                    
                    echo "\\n===== Deployment Status ====="
                    kubectl get pods,svc -n webapps
                '''
            }
        }
    }

    post {
        always {
            sh 'kubectl get all -n webapps || true'
        }
    }
}
