pipeline {
    agent { label 'JDK_8' }
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
    parameters {
        choice(name: 'GOAL', choices: ['package', 'clean package', 'install', 'clean install'], description: 'This is maven goal')
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
                sh script: "mvn ${params.GOAL}"
            }

        }
        stage('report') {
            steps {
                junit testResults: '**/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: '**/target/gameoflife.war'
            }
        }
    }
    post {
        success {
            mail subject: '${JOB_NAME}: has completed with success',
                 body: 'your project is effective \n Build Url ${BUILD_URL}',
                 to: 'jh@gmail.com'
        }
        failure {
            mail subject: '${JOB_NAME}:: has completed with failed',
                 body: 'your project is defective \n Build Url ${BUILD_URL}',
                 to: 'jh@gmail.com'
        }
    }
}