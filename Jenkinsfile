pipeline {
    agent any

   stages {       
              
        stage('Deploy to UAT env') {
            steps {
                    withKubeConfig([credentialsId: 'k8suser', serverUrl: 'https://technet-k8s.hds-cloudconnect.com:6443']) {                    
                    sh '''  
                        if kubectl describe service weather-app-uat -n demo-env-uat; then
                            echo "Service already exists, delete first"
                            kubectl delete service weather-app-uat -n demo-env-uat
                        fi
                        if kubectl describe deployment weather-app-uat -n demo-env-uat; then
                            echo "Application already exists, delete first"
                            kubectl delete deployment weather-app-uat -n demo-env-uat
                        fi                        
                        echo "deploy to K8S"                                  
                        sed -i.bak 's#weather-app:latest#technet-k8s.hds-cloudconnect.com:12018/weather-app:latest#' ./*.yaml
                        kubectl --namespace=demo-env-uat apply -f ./deployment.yaml
                        kubectl --namespace=demo-env-uat apply -f ./service.yaml                                                        
                        '''                
                    }
               }
          }
     }
}
