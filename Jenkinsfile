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
        stage("Paso 4: Springboot maven sleep 20"){
            steps {
                script{
                    sh "nohup bash ./mvnw spring-boot:run  & >/dev/null"
                    //sh "sleep 20 && curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'"
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

        stage("Paso 5: Detener Spring Boot"){
            steps {
                script{
                    sh '''
                        echo 'Process Spring Boot Java: ' $(pidof java | awk '{print $1}')  
                        sleep 20
                        kill -9 $(pidof java | awk '{print $1}')
                    '''
                }
            }
        }
    }
}