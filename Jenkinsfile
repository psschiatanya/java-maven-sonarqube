    pipeline {
    agent any
    triggers {
        pollSCM '* * * * *'
    }    
   
    tools {
           maven 'Maven'
            jdk 'JAVA_11'
            git 'GIT'
            
          }

    stages {

        stage ("Step0: Tools initialization")
                {
                steps {
                        sh "mvn --version"
                        sh "java -version"
                        sh "git --version"
                        sh "ansible --version"
                
                      }
                }
        
        stage ("Step1: Git Checkout")
                {
                steps {
                        echo "Step1: Git Checkout"
                        git url: 'https://github.com/psschiatanya/hello-world-1.git', branch: 'master'
                
                      }
                }
        stage ("Step2: Maven Build")
                {
                steps {
                        echo "Step2: Maven Build"
                        sh 'mvn -B -DskipTests clean install'
                
                      }
                }
        stage ("Step3: Copy the war")
                {
                steps {
                        echo "Step3: Copy the war"
                        sh 'ansible-playbook  /opt/ansible/playbook/project1/file_transfer.yml  -i /opt/ansible/playbook/project1/hosts'
                
                      }
                }
        stage ("Step4: Build the Docker & Push the Image")
                {
                steps {
                        echo "Step4: Build the Docker"
                         sh 'ansible-playbook  /opt/ansible/playbook/project1/docker_push.yml  -i /opt/ansible/playbook/project1/hosts'
                
                      }
                }   
        
        stage ("Step6: Deploy to Kubernetes ")
                {
                steps {
                  
                        echo "Step6: Deploy to Kubernetes "
                        sh 'ansible-playbook  /opt/ansible/playbook/project1/kube_deploy.yml  -i /opt/ansible/playbook/project1/hosts'
                      }
                } 
        }
         
    post{
            success{
                    emailext to: "psschiatanya@gmail.com",
                    subject: "Test Email",
                    body: "Test",
                    attachLog: true
                  }
        
           failure{
                    emailext to: "psschiatanya@gmail.com",
                    subject: "Test failed",
                    body: "Test",
                    attachLog: true
                }
        }
}

        
        
