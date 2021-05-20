pipeline {
    agent any
    stages {
        stage('Build Bundle') {
            steps {
                checkout scm
                dir('base-installer') {                    
                    sh 'sed -i s/%%RELEASE%%/${RELEASE}-${BUILD_NUMBER}/ roles/stereum-base/defaults/main.yml'
                    sh 'bundle-playbook -f ./playbook.yaml'
                    sh 'mv playbook.run base-installer-${RELEASE}-${BUILD_NUMBER}.run'
                    sh 'rm -f /var/jenkins_home/publish/*'
                    sh 'cp base-installer-${RELEASE}-${BUILD_NUMBER}.run /var/jenkins_home/publish'
                }                
            }
        }
        stage('Build Launcher') {
            steps {
                checkout scm                
                dir('launcher') {                    
                    sh 'sed -i s/%%RELEASE%%/${RELEASE}/ launcher.go'
                    sh 'go build'
                    sh 'cp launcher /var/jenkins_home/publish'
                }                
            }
        }
        stage('Push') {
            steps {
                // TODO: build windows binary and copy to share
                sh '/var/jenkins_home/release_publish.sh'
            }
        }        
    }
    post { 
        always { 
            cleanWs()
        }
    }
}