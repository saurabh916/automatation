pipeline {

    agent any
    environment {
        LOG_JUNIT_RESULTS = 'true'
    }
    stages {
        stage('Checkout Codebase'){
            steps{
                cleanWs()
               // sh "git clone https://github.com/saurabh916/automatation.git"
               // sh "mvn clean -f automation"
                checkout scm: [$class: 'GitSCM', branches: [[name: '*/master']],userRemoteConfigs:
                [[url: 'https://github.com/saurabh916/automatation.git']]]
            }
        }
        
        stage('check path') {
            steps {
              dir('src') {
                  sh "pwd"
                  sh'ls -la'
              }
            }
        }
        
    stage('Build'){
            steps{
                sh 'mkdir lib'
                sh 'cd lib/ ; wget https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.7.0/junit-platform-console-standalone-1.7.0-all.jar'
                sh "pwd"
                dir('src') {
                  sh "pwd"
                  sh'ls -la'
              //    sh 'javac -cp "../lib/junit-platform-console-standalone-1.7.0-all.jar" CarTest.java Car.java App.java'  
                 }
                sh "pwd"
            //    sh'ls -la'
                sh 'cd src ; javac -cp "../lib/junit-platform-console-standalone-1.7.0-all.jar" CarTest.java Car.java App.java'
            }
        }
        
        stage('Test'){
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                    sh 'cd src/ ; java -jar ../lib/junit-platform-console-standalone-1.7.0-all.jar -cp "." --select-class CarTest --reports-dir="reports"'
                }
                junit 'src/reports/*-jupiter.xml'
                influxDbPublisher(selectedTarget: 'InfluxPipelineData')
            }
        }
        

        stage('Deploy'){
            steps{
                sh 'cd src/ ; java App' 
            }
        }
    }

}        
