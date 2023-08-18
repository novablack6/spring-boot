def TCONST1 = 'top-const-1'

pipeline {
    agent { 
        label "cts"
    }

    options {
        timestamps()
        withEnv(["OENV1=options-env-variable-1"])
    }

    environment {
        PENV1 = "pipeline-env-valiable-1 + ${OENV1} "
        PENV2 = sh (
            script: 'echo pipeline-env-valiable-2',
            returnStdout: true
        ).trim()
        GRADLE = "/opt/gradle/bin/gradle"
    }

    stages {

        stage('Test Environment Variables') {

            environment {
                SENV1 = "stage-env-valiable-1 + ${OENV1}"
                SENV2 = sh (
                    script: 'echo stage-env-valiable-2',
                    returnStdout: true
                ).trim()
            }

            steps {
                echo "${TCONST1}"
                echo "${OENV1}"
                echo "${PENV1}"
                echo "${PENV2}"
                echo "${SENV1}"
                echo "${SENV2}"
            }
        }

        stage('Setup Gradle') {
            steps {
                sh '${GRADLE} -v'
            }
        }

        stage('Compile') {
            steps {
                sh '${GRADLE} clean testClasses'
            }
        }

        stage('Test') {
            steps {
                sh '${GRADLE} test'
            }
            post {
                always {
                    junit 'build/test-results/test/*.xml'
                }
            }
        }
    }
}
