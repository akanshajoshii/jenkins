pipeline {
    agent any
	    stages {
         stage('gitinstall') {
            steps {
                sh'''
                sudo yum install git -y
                '''
            }
        }
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/SanjayTomar22/pikachu.git' 
            }
        }
        stage('dockerinstall') {
            steps {
                sh'''
                sudo yum install docker -y
                sudo systemctl enable --now docker
                '''
            }
        }
        stage('dockerbuild') {
            steps {
                sh'''
                sudo docker build -t jenkipush .t
                '''
            }
        }
        stage('pushimage') {
            steps {
                sh'''
                sudo aws ecr get-login-password --region us-west-2 | sudo docker login --username AWS --password-stdin 637423194833.dkr.ecr.us-west-2.amazonaws.com
                sudo docker tag jenkipush:latest 637423194833.dkr.ecr.us-west-2.amazonaws.com/jenkipush:latest
                sudo docker push 637423194833.dkr.ecr.us-west-2.amazonaws.com/jenkipush:latest
               '''
            }
        }   
    }
}
            
        


