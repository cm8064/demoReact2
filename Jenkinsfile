pipeline {
    agent any

    tools {
        nodejs 'Node_24'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'step 1 - Checkout de rama'
                git branch: 'main', url: 'https://github.com/cm8064/demoReact2.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Step 2 - Compilación'
                dir('my-app') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Unit test') {
            steps {
                echo 'Step 3 - Pruebas unitarias'
                // Aquí deberías agregar los comandos para correr las pruebas y generar test-outputs.txt si aún no lo haces
            }
            post {
                always {
                    archiveArtifacts artifacts: 'test-outputs.txt', allowEmptyArchive: true
                }
            }
        }
    } //Stages

    post {
        success {
            echo "Pipeline finalizado con éxito"
        }
        failure {
            echo 'Pipeline con errores'
        }
        always {
            mail(
                to: 'carlos8064@gmail.com',
                subject: "Build Status: ${currentBuild.currentResult}",
                body: "Job: ${env.JOB_NAME}\nEstado: ${currentBuild.currentResult}\nURL: ${env.BUILD_URL}"
            )
        }
    }
}