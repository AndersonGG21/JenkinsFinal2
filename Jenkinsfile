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
                bat "curl http://Josue:josue2002@localhost:8080/job/FinalPruebas/job/DesplegarProduccion/build?token=Produccion"
                bat "curl http://Josue:josue2002@localhost:8080/job/FinalPruebas/job/NotificacionCorreca/build?token=FinalPruebas"
                bat "echo Tarea Desplegar en servidor de produccion Iniciada correctamente"
            }
            failure{
                bat "curl http://Josue:josue2002@localhost:8080/job/FinalPruebas/job/Notificacion/build?token=FinalPruebas"
                bat "echo Tarea notificar al correo Iniciada correctamente"
            }
        }
}
