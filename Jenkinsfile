pipeline {
    agent any
    stages {
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('Sonar') {
                    bat "mvn clean verify sonar:sonar"
                }
            }
        }
        stage("Quality gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
    post{
            success{
                bat "curl http://admin:12345@localhost:8080/job/finalPruebas/job/desplegarProduccion/build?token=produccion"
                bat "curl http://admin:12345@localhost:8080/job/finalPruebas/job/notificacionExito/build?token=finalPruebasDisparador"
                bat "echo Tarea Desplegar en servidor de produccion Iniciada correctamente"
            }
            failure{
                bat "curl http://admin:admin123@localhost:8080/job/finalPruebas/job/notificacionFallo/build?token=finalPruebasDisparador"
                bat "echo Tarea notificar al correo Iniciada correctamente"
            }
        }
}