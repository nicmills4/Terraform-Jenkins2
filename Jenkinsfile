pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

   agent  any
    stages {
        stage('checkout') {
            steps {
                 script{
                            git "https://github.com/nicmills4/Terraform-Jenkins2.git"
                    }
                }
            }

        stage ("terraform init") {
            steps {
                bat ("terraform init -reconfigure") 
            }
        }
        
        stage ("plan") {
            steps {
                bat ('terraform plan') 
            }
        }

        stage (" Action") {
            steps {
                echo "Terraform action is --> ${action}"
                bat ('terraform ${action} --auto-approve') 
           }
        }
    }

  }
