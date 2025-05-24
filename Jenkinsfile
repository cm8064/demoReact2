pipeline {
    agent any

tools{
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
                sh 'cd my-app'
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Unit test') {
            steps {
                echo 'Step 3 - Pruebas unitarias'
            }
            post{
                always{
                    archiveArtifacts artifacts: 'test-outputs.txt',
                    allowEmptyArchive: true
                }
            }
        }
    }
        post{
            success{
                echo "Pipeline finalizado con éxito"
            }
            failure{
                echo 'Pipeline con errores'
            }
            always {
                emailext ( subject: "Pipeline ${currentBuild.result}: ucp-app-react #${env.BUILD_NUMBER}", 
                body: """ Estado: ${currentBuild.result} URL Build: ${env.BUILD_URL} Detalles de Pruebas: ${env.BUILD_URL}testReport/ """, 
                to: 'carlos8064@gmail.com' )
        }
    }
