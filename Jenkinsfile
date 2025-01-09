pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://<usuario>@bitbucket.org/<usuario>/<nombre-del-repo>.git'
            }
        }

        stage('Build') {
            steps {
                // Si la landing page usa Node.js, puedes instalar dependencias y hacer build
                // Ejemplo: npm install y npm run build
                echo 'Building project...'
            }
        }

        stage('Deploy') {
            steps {
                // Aquí se puede hacer el despliegue, por ejemplo con rsync o scp a un servidor
                echo 'Deploying Landing Page'
                sh 'scp -r * usuario@servidor:/ruta/a/la/landing'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
