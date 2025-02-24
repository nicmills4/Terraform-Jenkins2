pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    agent any
    stages {
        stage('checkout') {
            steps {
                script {
                    dir("terraform") {
                        git "https://github.com/nicmills4/Terraform-Jenkins2.git"
                    }
                }
            }
        }
        stage('Plan') {
            steps {
                bat 'cd terraform && C:\Program Files\terraform\terraform.exe init'
                bat 'cd terraform && C:\Program Files\terraform\terraform.exe plan -out tfplan'
                bat 'cd terraform && C:\Program Files\terraform\terraform.exe show -no-color tfplan > tfplan.txt'
            }
        }
        stage('Approval') {
            when {
                not {
                    expression {
                        params.autoApprove == true
                    }
                }
            }
            steps {
                script {
                    def plan = readFile 'terraform/tfplan.txt'
                    input message: "Do you want to apply the plan?",
                        parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                }
            }
        }
        stage('Apply') {
            steps {
                bat 'cd terraform && C:\Program Files\terraform\terraform.exe apply -input=false tfplan'
            }
        }
    }
}
 