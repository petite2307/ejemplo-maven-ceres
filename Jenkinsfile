import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any
    stages {
        stage("Paso 1: Compilar"){
            steps {
                script {
                env.STAGE='Compilar'
                sh "echo 'Compile Code!'"
                sh "./mvnw clean compile -e"
                }
            }
        }
        stage("Paso 2: Testear"){
            steps {
                script {
                env.STAGE='Testear'
                sh "echo 'Test Code!'"
                sh "./mvnw clean test -e"
                }
            }
        }
        
        stage("Paso 3: Build .Jar"){
            steps {
                script {
                env.STAGE='Build .Jar'
                sh "echo 'Build .Jar!'"
                sh "./mvnw clean package -e"
                }
            }
        }
        stage("Paso 4: Análisis SonarQube"){
            steps {
                script {
                env.STAGE='Analisis Sonar'
                
                withSonarQubeEnv('sonarqube') {
                    sh "echo 'Calling sonar Service in another docker container!'"
                    // Run Maven on a Unix agent to execute Sonar.
                    sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=custom-project-key'
                }
                    
                }
            }
        stage("Paso 5: Testeo con Newman"){
            steps {
                script{
                    sh "sleep 20 && newman run /tmp/ejemplo-maven.postman_collection.json"
                }
            }
        }
    }
}