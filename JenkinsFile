node{
    
    stage("Code Checkout"){
       git'https://github.com/rutwikdadgal/capstone-insurance_me-project.git'
    }
    
    stage("Code build"){
        sh 'mvn clean package'
    }
    
    stage("Containerize"){
        sh 'docker build -t rutwikd/insure_me:1.0 .'
    }
    
    stage("push to dockerhub"){
        withCredentials([string(credentialsId: 'docker-hub-creds', variable: 'dockerhubpassword')]) {
            sh "docker login -u rutwikd -p ${dockerhubpassword}"
            sh 'docker push rutwikd/insure_me:1.0'
        }
        
    }
    
    stage("Deploy"){
       ansiblePlaybook become: true, credentialsId: 'ansible_ssh', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts/', playbook: 'configure-test-server', vaultTmpPath: ''
    }
}