pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('Aws_jenkins_users_cred') // Set this to the Jenkins credentials ID for your AWS access key
        AWS_SECRET_ACCESS_KEY = credentials('Aws_jenkins_users_cred') // Set this to the Jenkins credentials ID for your AWS secret key
        TF_VAR_region = 'us-west-2' // Optional: Set this to your AWS region
        PATH="/var/jenkins_home:$PATH"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/kumardevesh08/devops_projects.git', branch: 'dev'
            }
        }

        stage('Install Terraform') {
            steps {
                sh '''
                if ! command -v terraform &> /dev/null
                then
                    curl -O https://releases.hashicorp.com/terraform/1.5.3/terraform_1.5.3_linux_amd64.zip
                    unzip terraform_1.5.3_linux_amd64.zip
                    mv terraform /var/jenkins_home/
                fi
                '''
            }
        }

        stage('Initialize Terraform') {
            steps {
                dir('/var/jenkins_home/'){
                    sh 'terraform init'

                }
                
            }
        }

        stage('Plan') {
            steps {
                dir('/var/jenkins_home/'){
                    sh 'terraform plan'

                }
            }
        }

        stage('Apply') {
            steps {
                dir('/var/jenkins_home/'){
                    sh 'terrform apply -auto-approve'

                }
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/*.tf', allowEmptyArchive: true
            cleanWs()
        }
    }
}
