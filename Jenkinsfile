pipeline{
    agent any

    parameters {
            choice choices: ['chrome', 'firefox'], name: 'Browser'
        }

    stages{
        stage("Start Grid"){
            steps{
                bat "docker-compose -f grid.yaml up --scale ${params.Browser}=2 -d"
            }
        }
        stage("Execute Test Suites"){
            steps{
                bat "docker-compose -f test-suites.yaml up"
            }
        }
        }

    post{
        always{
            bat "docker-compose -f grid.yaml down"
            bat "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'results/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'results/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
}