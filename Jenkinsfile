pipeline {
    agent any

    stages {
        stage('Compile Code') {
            steps {
                sh "./mvnw.cmd clean compile -e"
            }
        }
        stage('Test Code') {
            steps {
                sh "./mvnw.cmd clean test -e"
            }
        }
        stage('Jar Code') {
            steps {
                sh "./mvnw.cmd clean package -e"
            }
        }
        stage('Run Jar') {
            steps {
                sh "nohup bash mvnw.cmd spring-boot:run &"
            }
        }
        stage('Good Bye') {
            steps {
                echo 'Good Bye Usach Ceres'
            }
        }
    }
}