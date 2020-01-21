pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building'
                sh 'python3 --version'
                sh 'python3 -m py_compile Hello_World/app.py'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                sh 'ssh -i "~/ssh/clouderide-kp.pem" ec2-user@10.0.1.1 rm -rf /var/cloud/temp_deploy/Hello_World/'
                sh 'ssh -i "~/ssh/clouderide-kp.pem" ec2-user@10.0.1.1 mkdir -p /var/cloud/temp_deploy'
                sh 'scp -i "~/ssh/clouderide-kp.pem" -r Hello_World ec2-user@10.0.1.1:/var/cloud/temp_deploy/Hello_World/'

                sh 'ssh -i "~/ssh/clouderide-kp.pem" ec2-user@10.0.1.1 "rm -rf /var/app/demo/Hello_World/ && mv /var/cloud/temp_deploy/Hello_World/ /var/app/demo/Hello_World/"'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
