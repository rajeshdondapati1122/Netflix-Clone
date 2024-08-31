pipeline {
    agent any
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages {
        stage('clean') {
            steps{
                cleanWs()
            }
                
        }
    
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/rajeshdondapati1122/Netflix-Clone.git'
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=netflix \
                    -Dsonar.projectKey=netflix '''
                }
            }
        }
        
        stage('trivy') {
            steps {
                sh'trivy fs .'
            }
        }
        
        stage('docker image'){
            steps{
                sh 'docker build -t netflix:v1 .'
            }
        }
        
        stage('trivy image'){
            steps{
                sh 'trivy image netflix:v1'
            }
        }
        
        stage('run'){
            steps{
                sh 'docker run -itd --name container -p 4000:80 netflix:v1'
                
            }
        }
        
    }
}
