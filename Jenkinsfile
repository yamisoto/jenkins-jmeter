pipeline {
    agent any
    tools {
      maven 'maven3'
    }
    stages {
        stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yamisoto/jenkins-jmeter.git']])
            }    
        }
        stage('Build'){
            steps{
                echo 'Building Maven project'
                sh 'mvn compile'
            }
        }
        stage('jmeter') {
            steps {
                echo 'Running jmeter test'
                
                sh '''/opt/jmeter/bin/jmeter -jjmeter.save.saveservice.output_format=xml -n -t ${WORKSPACE}/simplilearn.jmx -l ${WORKSPACE}/report.jtl'''
            }
        }
        stage('publish jmeter report'){
            steps{
                perfReport filterRegex: '', showTrendGraphs: true, sourceDataFiles: 'report.jtl'
            }
        }
                stage('NotifyByEmail'){
            steps{
                emailext attachLog: true, attachmentsPattern: '${WORKSPACE}/simplilearn.jmx', body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                Check console output at $BUILD_URL, Artifacts list at $RUN_ARTIFACTS_DISPLAY_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ademoye@gmail.com'
            }
        }
    }
 }
