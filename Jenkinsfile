
pipeline {
    agent any

    stages {
        stage('Check Connection') {
                
            steps {
                script{

                        sshagent(['79a4420b-3683-4745-a8a1-9e27b796f7b3']) {
                            sh '''
                            ssh alamin@45.120.115.246 "echo "server running....""
                            '''
               
                        }
              
                    }
                   
                }

            post {
                success {
                       notifyDiscord("Connection success!")
                }
                failure {
                     notifyDiscord("Connection failed!")
                }
            }
            
        }

        stage('update code') {
            steps {
                script{

                       sshagent(['79a4420b-3683-4745-a8a1-9e27b796f7b3']) {
                            sh '''
                            ssh alamin@45.120.115.246 "
                            cd /home/alamin/storage/dcl-project/riyad-deepchainlab-com  &&
                            sudo git reset .  && 
                            sudo git clean -df &&
                            sudo git stash &&
                            sudo git pull
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }

             post {
                success {
                       notifyDiscord("update code  success!")
                }
                failure {
                     notifyDiscord("update code failed!")
                }
            }
       
        }
 

         stage('deploy code') {
            steps {
                script{

                       sshagent(['79a4420b-3683-4745-a8a1-9e27b796f7b3']) {
                            sh '''
                            ssh alamin@45.120.115.246 "
                                    cd /home/alamin/storage/dcl-project/riyad-deepchainlab-com &&
                                    sudo docker compose up -d --build &&
                                    sudo docker system prune -af
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }

              post {
             
                success {
                       notifyDiscord("deploy success!")
                }
                failure {
                     notifyDiscord("deploy failed!")
                }
            }
        }

        
        
    }

}
def notifyDiscord(message) {
    def discordWebhookUrl = 'https://discord.com/api/webhooks/1145653952562597918/b9wR-EbUYiBOOsn3jHGxq1Cg45sGiz45yfo_ASMtAXZzMmhpyvvzQCEl7EL-OBrwUXgS'
    sh "curl -X POST -H 'Content-Type: application/json' -d '{\"content\":\"${message}\"}' ${discordWebhookUrl}"
}
    
