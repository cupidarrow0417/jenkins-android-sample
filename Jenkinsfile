pipeline {
    agent any

    parameters {
        string(
                name: 'PERSON',
                defaultValue: 'Mr Jenkins',
                description: 'Who should I say hello to?'
        )

        choice(
                name: 'BRANCH',
                choices: 'prod\ndev',
                description: 'Choice Branch'
        )

        booleanParam(
                name: 'CAN_DANCE',
                defaultValue: true,
                description: 'Checkbox parameter'
        )
    }

    stages {
        stage('Initialize') {
            steps {
                echo 'Initialize...'
                echo "PERSON=${params.PERSON} BRANCH=${params.BRANCH} CAN_DANCE=${params.CAN_DANCE}"

                withEnv(['DISABLE_AUTH=true', 'DB_ENGINE=sqlite']) {
                    echo "${env.DB_ENGINE} ${env.DISABLE_AUTH}"
                    sh 'echo $DB_ENGINE $DISABLE_AUTH'
                }
            }
        }

        stage('Build Develop APK') {
            when {
                branch 'master'
            }
            steps {
                echo 'Building Develop APK...'
                sh './gradlew clean assembleDevDebug'
            }
        }

        stage('Build Beta APK') {
            when {
                branch 'beta'
            }
            steps {
                withCredentials([string(credentialsId: '0c74f122-d8d0-4cab-9cea-8a3b7d76a435', variable: 'SECRET_KEY')]) {
                    echo 'Building Beta APK...'
                    sh './gradlew clean assembleBetaDebug'
                }

            }
        }

        stage('Build Prod APK') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([string(credentialsId: '0c74f122-d8d0-4cab-9cea-8a3b7d76a435', variable: 'SECRET_KEY')]) {
                    echo 'Building Production APK...'
                    sh './gradlew clean assembleProd'
                }
            }
        }

        stage('Sign Prod APK') {
            when {
                branch 'master'
            }
            steps {
                echo 'Sign APK'
                signAndroidApks(
                        keyStoreId: "94ddde4c-7b9e-459f-bd15-11eda63541de",
                        keyAlias: "tomczhen",
                        apksToSign: "**/*-prod-release-unsigned.apk",
                        archiveSignedApks: false,
                        archiveUnsignedApks: false
                )
            }
        }

        stage('Upload') {
            steps {
                echo 'Upload'
                archiveArtifacts(onlyIfSuccessful: true, artifacts: 'app/build/outputs/apk/*.apk')
            }
        }
        stage('Report') {
            steps {
                echo 'Report'
            }
        }
    }

    post {
        always {
            echo 'Always Echo!'
        }
        success {
            echo 'Build Success!'
        }
        failure {
            echo 'Build Failure!'
        }
        changed {
            echo 'Build Status Changed!'
        }
        unstable {
            echo 'Test Failure!'
        }
        aborted {
            echo 'Build Aborted!'
        }
    }
}