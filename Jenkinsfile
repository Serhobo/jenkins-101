pipeline {
    agent { 
        node {
            label 'docker-agent-python'
            }
      }
    triggers {
        pollSCM '*/5 * * * *'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building.."
                sh '''
                cd myapp
                pip install -r requirements.txt
                '''
            }
        }
        stage('Deploy to GCE') {
            steps {
                script {
                    withCredentials([file(credentialsId: '4f9f3476-449a-49fe-ab80-e775559a05fb', variable: 'GOOGLE_CREDENTIALS')]) {
                        sh "echo '$GOOGLE_CREDENTIALS' > /tmp/key.json"
                        sh 'gcloud auth activate-service-account --key-file=/tmp/key.json'
                        sh "gcloud config set project $in-class-lab-3-405222"
                        sh "gcloud compute instances create $temp_in_class_lab_3_thing --image-family=$ubuntu-2004-lts --image-project=$ubuntu-os-cloud"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                cd myapp
                python3 hello.py
                python3 hello.py --name=SerHobo
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
}
