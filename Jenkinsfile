pipeline {
    agent any
    stages {
        stage('PULL GIT'){
            steps{
              checkout([$class: 'GitSCM', branches: [[name: '**']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/alahoti4/PROJ-CHECK']]])  
            }
            
        }
        stage('Build Docker image'){
            steps{
                script{
               sh 'docker build -t alahoti4/202109021231 .' 
                }
            }
            
        }
        stage('Push docker image'){
        steps{
            script{
                withCredentials([string(credentialsId: 'alahoti4', variable: 'dockerhubpwd')]) {
                sh 'docker login -u alahoti4 -p ${dockerhubpwd}'
}
                 sh 'docker push alahoti4/202109021231'
            }
                
            }
        }
        
        stage('Deploy to Kubernetes'){
            steps{
                script{
                    try{
                             sh 'kubectl apply -f train-schedule-kube.yml'
                             sh 'kubectl apply -f train-schedule-kube-canary.yml'
                    }
                    catch(error)
                    {
                           sh 'kubectl apply -f train-schedule-kube.yml'
                           sh 'kubectl apply -f train-schedule-kube-canary.yml'
                        
                    }
                    
                         }
                    }
                   
                    }
                }
       
    }
}
