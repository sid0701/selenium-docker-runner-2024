pipeline{
    agent any

    parameters {
            choice choices: ['chrome', 'firefox'], name: 'Browser'
        }

    stages{
        stage("Start Grid"){
            steps{
                sh "docker-compose -f grid.yaml up --scale ${params.Browser}=1 -d"
            }
        }
        stage("Execute Test Suites"){
            steps{
                sh "docker-compose -f test-suites.yaml up --pull=always"
                script{
                    if(fileExists('results/flight-reservation/testng-failed.xml') || fileExists('results/vendor-portal/testng-failed.xml'))
                        error("Few test cases have failed!!Please have a look.")
                }
            }
        }
        }

    post{
        always{
            sh "docker-compose -f grid.yaml down"
            sh "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'results/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'results/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
}