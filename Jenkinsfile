pipeline {
    agent any

    environment {
        EC2_USER = "ubuntu"  // Replace with your EC2 user
        EC2_HOST = "3.15.218.116"  // Replace with your EC2 IP
        EC2_KEY = credentials('ssh-key')  // Your private key in Jenkins credentials
        PROJECT_DIR = "/home/ubuntu/my_django_project"  // Path to your Django project on the EC2 instance
    }

    stages {
        stage('Update Code on EC2') {
            steps {
                script {
                    // Use SSH to pull the latest code and restart the server
                    sshagent (credentials: ['ssh-key']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ubuntu@3.15.218.116 '
                                cd /home/ubuntu/my_django_project
                                git pull origin main
                                source myworld/bin/activate
                                python3 -m pip install -r requirements.txt
                                sudo systemctl restart apache2  # or the service you're using for the web app
                            '
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Deployment succeeded!"
        }
        failure {
            echo "Deployment failed."
        }
    }
}
