pipeline {
    agent { label 'JDK_8' }
    options {
        retry(3)
        timeout(time: 30, unit: 'MINUTES')
    }
    tools {
        jdk 'java8'
        maven 'MAVEN_3.6'
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
       stage('build and package') {
            steps {
                 rtMavenDeployer (
                    id: "Gol_DEPLOYER",
                    serverId: "jfrog",
                    releaseRepo: 'gol-libs-release-local',
                    snapshotRepo: 'gol-libs-snapshot-local'
                )
                rtMavenRun (
                    tool: 'MAVEN_3.6', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "Gol_DEPLOYER"
                    //,
                    //buildName: "${JOB_NAME}",
                    //buildNumber: "${BUILD_ID}"
                )
                rtPublishBuildInfo (
                    serverId: "jfrog"
                )
            }
        }
        stage('reporting') {
            steps {
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
            }
        }
    }

}
