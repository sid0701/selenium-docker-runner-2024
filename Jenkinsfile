pipeline{
    agent any

    parameters {
            choice choices: ['chrome', 'firefox'], name: 'Browser'
        }

    stages{
        stage("Start Grid"){
            steps{
                sh "docker compose -f grid.yaml up --scale ${params.Browser}=1 -d"
            }
        }
        stage("Execute Test Suites"){
            steps{
                parameters{
                    choice choices: ['flight-reservation.xml', 'vendor-app.xml'], name: 'TEST_SUITE'
                    }
                sh "TEST_SUITE=${params.TEST_SUITE} docker compose -f test-suites.yaml up --pull=always"
                script{
                    if(fileExists('results/test-suite/testng-failed.xml'))
                        error("Few test cases have failed!!Please have a look.")
                }
            }
        }
        }

    post{
        always{
            sh "docker compose -f grid.yaml down"
            sh "docker compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'results/test-suite/emailable-report.html', followSymlinks: false
        }
    }
}