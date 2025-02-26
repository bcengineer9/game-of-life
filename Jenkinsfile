pipeline {
    agent { label 'india' }
    
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/bcengineer9/game-of-life.git',
                    branch: 'notifications'
            }
        }
        
        stage('package') {
            tools {
                jdk 'JAVA-'  // Ensure this JDK is configured in Jenkins
            }
            steps {
                sh 'mvn clean package'  // Added clean for a fresh build
            }
        }
        
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife.war', onlyIfSuccessful: true
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
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} failed",
                body: "Use this URL ${BUILD_URL} for more info",
                to: 'team-manager-qt@qt.com',
                from: 'devops@qt.com'
        }
    }
}
