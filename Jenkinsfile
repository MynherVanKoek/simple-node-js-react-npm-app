pipeline {
    agent any
    // agent {
    //     docker {
    //         image 'node:6-alpine' 
    //         args '-p 3000:3000' 
    //     }
    // }
    environment {
        CI = 'true' 
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
                // sh 'sudo npm install'
                sh "npm run lint"
                recordIssues enabledForFailure: true, tool: esLint(pattern: '**/lintresult.xml')
                // recordIssues enabledForFailure: true, tool: esLint()

                // sh "npm run inspect:vulnerabilities"
                // recordIssues enabledForFailure: true, tool: issues(pattern: '**/vulnresult.json')

                dependencyCheck
                dependencyCheckPublisher
            }
        }
        // stage('Lint') {

        //     steps{

        //         sh "npm run lint"
        //         recordIssues enabledForFailure: true, tools: esLint(pattern: '/tmp/jenkins/lint.xml')
        //         // recordIssues enabledForFailure: true, tools: esLint()

        //     }

        // }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}