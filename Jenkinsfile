pipeline{
    agent any
    tools {
        maven 'M2_HOME'
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
    }
}
    
