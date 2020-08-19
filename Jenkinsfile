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
                sh "npm run inspect:all"
                recordIssues enabledForFailure: true, tool: esLint(pattern: '**/lintresult.xml')
                // recordIssues enabledForFailure: true, tool: issues(pattern: '**/vulnresult.json')
            }
        }
        stage('Disply licenses') {
            sh '''
                git clone https://github.com/derekeder/csv-to-html-table.git
                cp licresult.csv csv-to-html-table/data/Health\ Clinics\ in\ Chicago.csv
            '''
            publishHTML([
                allowMissing: false, 
                alwaysLinkToLastBuild: false, 
                keepAll: false, 
                reportDir: 'csv-to-html-table', 
                reportFiles: 'index.html', 
                reportName: 'Licenses', 
                reportTitles: '']
            )
        }
        // stage('Lint') {

        //     steps{

        //         sh "npm run lint"
        //         recordIssues enabledForFailure: true, tools: esLint(pattern: '/tmp/jenkins/lint.xml')
        //         // recordIssues enabledForFailure: true, tools: esLint()

        //     }

        // }
        // stage('Dependency Check') {
        //     steps {
        //         dependencyCheck additionalArguments: "--scan ${WORKSPACE} --out ./dependency-check-report.xml --format XML", odcInstallation: 'DepCheck5.3.2'
        //         dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         sh './jenkins/scripts/test.sh'
        //     }
        // }
        // stage('Deliver') {
        //     steps {
        //         sh './jenkins/scripts/deliver.sh'
        //         input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //         sh './jenkins/scripts/kill.sh'
        //     }
        // }
    }
}