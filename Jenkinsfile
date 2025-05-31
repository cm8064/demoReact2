pipeline {
    agent any

    tools {
        nodejs 'Node_24'
        sonarScanner 'SonarQubeScanner' // Configurado en Global Tools
    }

    environment {
        SONAR_PROJECT_KEY = 'ucp-app-react'
        SONAR_PROJECT_NAME = 'UCP React App'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'step 1 - Checkout de rama'
                git branch: 'main', url: 'https://github.com/cm8064/demoReact2.git'
            }
        }

        // Nueva etapa: Análisis de SonarQube
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                    -Dsonar.projectName=${SONAR_PROJECT_NAME} \
                    -Dsonar.sources=src \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=${SONAR_AUTH_TOKEN} \
                    -Dsonar.javascript.node=${NODEJS_HOME}/bin/node \
                    -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Step 2 - Compilación'
                dir('my-app') {
                    sh 'npm install'
                    sh 'npm run build'
                    sh 'npm run test:coverage' // Asegúrate de que tu package.json tenga este script
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
            
            script {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Calidad no aprobada: ${qg.status}"
                }
            }
            
            mail(
                to: 'carlos8064@gmail.com',
                subject: "Build Status: ${currentBuild.currentResult}",
                body: "Job: ${env.JOB_NAME}\nEstado: ${currentBuild.currentResult}\nURL: ${env.BUILD_URL}"
            )
        }
    }
}