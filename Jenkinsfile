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
                mkdir -p /home/ashay/testlocal && cp -r $(pwd) /home/ashay/testlocal && chown -R ashay /home/ashay/testlocal
                scp -r -i ${key} /home/ashay/testlocal ${destination}:/home/ashay/test/
                echo "successfully copied git files to test server"
                '''              
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
                 mkdir -p /home/ashay/prodlocal && cp -r $(pwd) /home/ashay/prodlocal && chown -R ashay /home/ashay/prodlocal
                 scp -r -i ${key} /home/ashay/prodlocal ${destination}:/home/ashay/prod/
                 echo "successfully copied git files to prod server"
                '''  
            }
        }
    }
}