pipeline{
    agent any
    tools {
        maven 'M2_HOME'
      }
    environment {
      DOCKER_TAG = getVersion()
    }
    stages{
        stage('SCM'){
            steps{
                git credentialsId: 'github', 
                    url: 'https://github.com/sampathraj251/dockeransiblejenkins'
            }
        }
       stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        } 
      stage('Docker Build'){
            steps{
                sh "docker build . -t sampathgorre/myapp:${DOCKER_TAG} "
            }
        }
       stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerHubPassword')]) {
                    sh "docker login -u sampathgorre -p ${dockerHubPassword}"
                     }
                
                sh "docker push sampathgorre/myapp:${DOCKER_TAG} "
            }
        }
    
         stage('Docker Deploy'){
            steps{
              ansiblePlaybook credentialsId: 'docker-host-dev', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
    } 
    
}
    
def getVersion(){
   def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
    
}
