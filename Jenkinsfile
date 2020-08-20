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
            steps {
                sh '''cat << EOF > conversion.py
                    !#/usr/bin/env python3.7
                    import pandas as pd

                    df = pd.read_csv("licresult.csv")
                    df.to_html("licenses.html")
                    EOF
                '''
                sh 'cat conversion.py'
                // publishHTML([
                //     allowMissing: false, 
                //     alwaysLinkToLastBuild: false, 
                //     keepAll: false, 
                //     reportDir: 'csv-to-html-table', 
                //     reportFiles: 'index.html', 
                //     reportName: 'Licenses', 
                //     reportTitles: 'Licenses']
                // )
            }
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