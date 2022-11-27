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
            post{
                success {
                    slackSend color: 'good', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion exitosa"

                }
                failure{
                    slackSend color: 'danger', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion fallida en stage ${env.STAGE}"
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
			post{
                success {
                    slackSend color: 'good', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion exitosa"

                }
                failure{
                    slackSend color: 'danger', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion fallida en stage ${env.STAGE}"
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
            post {
                //record the test results and archive the jar file.
                success {
                    archiveArtifacts artifacts:'build/*.jar'
                    slackSend color: 'good', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion exitosa"
                }
                
                failure{
                     slackSend color: 'danger', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion fallida en stage ${env.STAGE}"
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
             post{
                success {
                    slackSend color: 'good', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion exitosa"

                }
                failure{
                    slackSend color: 'danger', message: "Maritza Cornejo - ${env.JOB_NAME} - Ejecucion fallida en stage ${env.STAGE}"
                }
            }
        }
    }
}