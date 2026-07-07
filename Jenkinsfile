pipeline {
    agent any

    tools {
        nodejs "Node25" // Configura una instalación de Node.js en Jenkins
        // dockerTool 'DockerTool'  // Se comenta para usar el cliente Docker instalado en el sistema (evita descargar la versión obsoleta 1.29)
    }

    stages {
        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor Node.js') {
            steps {
                sh '''
                    # Detener y eliminar cualquier contenedor previo
                    docker stop hola-mundo-node || true
                    docker rm hola-mundo-node || true

                    # Ejecutar el contenedor de la aplicación
                    docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                '''
            }
        }
    }
}