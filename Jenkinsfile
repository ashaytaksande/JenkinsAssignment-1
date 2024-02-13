pipeline {
    agent any

    environment {
        branchName = "${env.GIT_BRANCH.split('/').size() == 1 ? env.GIT_BRANCH.split('/')[-1] : env.GIT_BRANCH.split('/')[1..-1].join('/')}"
        key = credentials('key')
        destination = 'ashay@ec2-3-82-152-142.compute-1.amazonaws.com'
    }
    stages {
        stage('Copy files to test server') {
            agent {
                label 'test'
            }
            when {
                expression {
                    branchName == 'test'
                }
            }
            steps {
                sh '''
                scp -r -i ${key} $(pwd)/ ${destination}:/home/ashay/test/
                '''
                sh ' echo "successfully copied git files to test server" '
               
            }
        }

        stage('Copy files to prod server') {
            agent {
                label 'prod'
            }
            when {
                expression {
                    branchName == 'prod'
                }
            }
            steps {
                 sh '''
                scp -r -i ${key} $(pwd)/ ${destination}:/home/ashay/prod/
                '''
                sh ' echo "successfully copied git files to prod server" '
            }
        }
    }
}