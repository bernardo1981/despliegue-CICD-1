Paso 1: Preparar el Repositorio de Bitbucket
Crear un repositorio en Bitbucket:

Ve a Bitbucket y crea un nuevo repositorio.
Si el repositorio es privado, asegúrate de que puedas acceder con las credenciales necesarias.
Clonar el repositorio de GitHub en tu repositorio de Bitbucket:

Clona el proyecto del repositorio de GitHub a tu máquina local:

bash
Copiar código
git clone https://github.com/bernardo1981/despliegue-CICD-1.git
Después, agrega el repositorio de Bitbucket como remoto en tu proyecto local:

bash
Copiar código
cd despliegue-CICD-1
git remote add bitbucket https://<usuario>@bitbucket.org/<usuario>/<nombre-del-repo>.git
Subir el Proyecto a Bitbucket:

Una vez que hayas agregado el remoto, sube el proyecto:
bash
Copiar código
git push bitbucket master
Paso 2: Preparar Jenkins
Instalar Jenkins:

Si no tienes Jenkins instalado, puedes seguir la guía oficial para instalar Jenkins en tu servidor.
Configurar Jenkins:

Asegúrate de tener instalado el plugin Git y Bitbucket Plugin en Jenkins:
Ve a Administrar Jenkins > Administrar Plugins.
Busca los plugins Git y Bitbucket y asegúrate de que estén instalados.
Configurar un Job en Jenkins para el Proyecto:

Crea un nuevo Job de tipo Freestyle:
Ve a Nuevo Job en Jenkins.
Selecciona Freestyle project.
Ponle un nombre descriptivo, por ejemplo: Deploy-LandingPage.
Haz clic en OK para continuar.
Configurar Repositorio Git de Bitbucket:

En la sección Source Code Management, selecciona Git.
En la URL del repositorio, coloca la URL del repositorio de Bitbucket:
bash
Copiar código
https://<usuario>@bitbucket.org/<usuario>/<nombre-del-repo>.git
Configurar las Credenciales:

Añade las credenciales de Bitbucket si es necesario:
Ve a Administrar Jenkins > Administrar Credenciales.
Añade las credenciales necesarias (usuario y contraseña o un token de acceso).
Vuelve al Job y selecciona las credenciales apropiadas.
Paso 3: Configurar el Pipeline de Jenkins (Opcional)
Si prefieres usar un pipeline de Jenkins en lugar de un Job freestyle, puedes configurar un archivo Jenkinsfile en tu repositorio de Bitbucket. Aquí te muestro un ejemplo básico de un Jenkinsfile para un despliegue simple de una landing page estática.

Crear un archivo Jenkinsfile en el repositorio:

Dentro de tu proyecto en Bitbucket, crea un archivo llamado Jenkinsfile en la raíz del proyecto. Este archivo define el pipeline de Jenkins.
Ejemplo de un Jenkinsfile básico para desplegar una landing page estática (HTML, CSS, JS):

groovy
Copiar código
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
Este Jenkinsfile realiza las siguientes etapas:
Checkout: Clona el repositorio desde Bitbucket.
Build: En este caso, es solo un echo, pero puedes agregar los pasos necesarios si es una página que requiere compilación (como un proyecto de Node.js).
Deploy: Realiza el despliegue al servidor de destino. Puedes usar herramientas como scp, rsync o configuraciones específicas para desplegar tu página en tu servidor.
Configurar el Job de Jenkins para usar el Jenkinsfile:

Si estás usando un Jenkinsfile, debes configurar el job para usarlo desde el repositorio:
En tu configuración del job en Jenkins, selecciona la opción Pipeline.
En Definition, selecciona Pipeline script from SCM.
En SCM, selecciona Git y coloca la URL de tu repositorio de Bitbucket.
Jenkins leerá automáticamente el archivo Jenkinsfile desde tu repositorio.
Paso 4: Configurar Webhooks en Bitbucket
Para automatizar el proceso de integración continua, puedes configurar un webhook en Bitbucket para que, cada vez que se haga un commit en el repositorio, Jenkins ejecute automáticamente el proceso de integración y despliegue.

Ve a tu repositorio en Bitbucket.
Ve a Repository settings > Webhooks.
Agrega un nuevo webhook con la URL de Jenkins, algo como:
bash
Copiar código
http://<jenkins-url>/job/<nombre-del-job>/build
Esto permitirá que cada vez que realices un push al repositorio, Jenkins automáticamente dispare el proceso de construcción y despliegue.
Paso 5: Verificación
Haz un cambio en tu landing page (por ejemplo, actualiza el archivo index.html).

Realiza un commit y push al repositorio de Bitbucket:

bash
Copiar código
git commit -m "Actualización de la landing page"
git push bitbucket master
Jenkins debería recibir la notificación del webhook y comenzar el proceso de CI/CD.

Verifica que el despliegue se haya realizado correctamente en tu servidor.

Resumen
Configura Bitbucket con tu repositorio y sube tu código.
Configura Jenkins para usar Bitbucket como fuente de código, con un Job Freestyle o Pipeline (Jenkinsfile).
Configura el Webhook en Bitbucket para que, al hacer un push, Jenkins ejecute el proceso de CI/CD automáticamente.
Despliega tu Landing Page: El proceso de CI/CD se encargará de construir y desplegar automáticamente tu landing page.
Con estos pasos, habrás creado un flujo de trabajo automatizado de CI/CD para tu proyecto de landing page usando Bitbucket y Jenkins.



