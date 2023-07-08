pipeline {
    agent any 

    tools { 
        maven 'jenkinsmaven'
        jdk 'javajenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '*****     PASO 1     *****'
                echo 'Clonando repositorio desde GitHub'
                git branch: 'main', url: 'https://github.com/spicke23/DevopsTestMerge.git'
            }
        }

        stage('Build') {
            steps {
                echo '*****     PASO 2     *****'
                echo 'Ejecutando Maven (limpieza de directorio y test de codigo)'
                script {
                    sh 'mvn clean test'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                script {
                    sh 'mvn package'
                }
                echo '*****     PASO 3     *****'
                echo 'Almacenamiento del artefacto en el directorio TARGET'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

#        stage('Test') {
#            steps {
#                echo 'Ejecutando test de Maven'
#                script {
#                    sh 'mvn test'
#                }
#            }
#        }

        stage('Sonar Scanner') {
            steps {
                echo 'Ejecutando analisis desde SonarQube'
                script {
                    def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                        sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://SonarQube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=ProyectoFinal-Gr4 -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/java/com/kibernumacademy/devops -Dsonar.tests=src/test/java/com/kibernumacademy/devops -Dsonar.language=java -Dsonar.java.binaries=."
                    }
                }
            }
        }
    }
}
