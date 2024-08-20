pipeline{
    agent none

    stages{
        stage("Start Grid"){
            steps{
                bat "docker-compose -f grid.yaml up --scale chrome=1 --scale firefox=1 -d"
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
        }
    }
}