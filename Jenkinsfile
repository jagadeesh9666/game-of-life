pipeline {
    agent { label 'java8' }
    options {
        retry(3)
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'java8'
        maven 'MAVEN_3.9'
    }
    stages {
        stage('code') {
            steps {
                git url: 'https://github.com/jagadeesh9666/game-of-life.git',
                    branch: 'master'
            }
        }
        stage('package') {
            steps {
                sh script: 'mvn clean package'
            }

        }
        stage('report') {
            steps {
                junit testResults: '**/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: '**/target/gameoflife.war'
            }

        }
    }
}