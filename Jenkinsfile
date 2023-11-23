pipeline {
    agent { 
        node {
            label 'docker-agent-python'
            }
      }
    environment {
        GOOGLE_CREDENTIALS = credentials('4f9f3476-449a-49fe-ab80-e775559a05fb')
        GCP_PROJECT_ID = 'in-class-lab-3-405222'
        GCE_INSTANCE_NAME = 'temp_in_class_lab_3_thing'
        GCE_IMAGE_FAMILY = 'ubuntu-2004-lts'
        GCE_IMAGE_PROJECT = 'ubuntu-os-cloud'
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
                        sh "echo '\$GOOGLE_CREDENTIALS' > /tmp/key.json"
                        sh 'gcloud auth activate-service-account --key-file=$WORKSPACE/keyfile.json'
                        sh "gcloud config set project $GCP_PROJECT_ID"
                        sh "gcloud compute instances create $GCE_INSTANCE_NAME --image-family=$GCE_IMAGE_FAMILY --image-project=$GCE_IMAGE_PROJECT"
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
