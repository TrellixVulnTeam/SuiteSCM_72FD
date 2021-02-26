pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'chmod +x install.sh'
                sh './install.sh'
            }
        }
        stage('Test') {
            steps {
                catchError {
                    sh 'docker run --name my_test_48 my_test3 --url ${URL} --executor ${EXECUTOR} --browser ${BROWSER} --bversion ${BVERSION} -n ${NODES}'
                }
            }
         }
        stage('Copy_allure') {
            steps {
//                 sh 'docker run --name my_test_35 my_test1 --url ${URL} --executor ${EXECUTOR} --browser ${BROWSER} --bversion ${BVERSION} -n ${NODES}'
                   sh 'docker cp my_test_48:/app/allure-result /var/jenkins_home/workspace/test2/allure-results'
//                 sh 'docker system prune -f'
            }
        }
    }

    post {
        always {

            script {
                allure([
                        includeProperties: false,
                        jdk: '',
                        properties: [],
                        reportBuildPolicy: 'ALWAYS',
                        results: [[path: '../allure-report']]
                ])
            }

            cleanWs()
        }
    }
}