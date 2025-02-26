pipeline {
    agent { label 'india' }
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/bcengineer9/game-of-life.git',
                    branch: 'notifications'
            }
        }
        stage('package') {
            tools {
                jdk 'JAVA-8'
            }
            steps {
                sh "mvn package"
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
    post {
        success {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                body: "Use this URL ${BUILD_URL} for more info",
                to: 'team-all-qt@qt.com',
                from: 'devops@qt.com'
        }
        failure {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failed",
                body: "Use this URL ${BUILD_URL} for more info",
                to: "${GIT_AUTHOR_EMAIL}",
                from: 'devops@qt.com'
        }
    }

