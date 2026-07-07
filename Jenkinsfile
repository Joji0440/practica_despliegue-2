pipeline {
    agent any

    environment {
        // Agrega la carpeta 'bin' del workspace al PATH para encontrar el ejecutable de Docker
        PATH = "${WORKSPACE}/bin:${env.PATH}"
    }

    tools {
        nodejs "Node25" // Configura una instalación de Node.js en Jenkins
    }

    stages {
        stage('Instalar Docker CLI') {
            steps {
                sh '''
                    if ! command -v docker &> /dev/null; then
                        echo "Docker no está en el PATH. Descargando cliente Docker CLI moderno..."
                        mkdir -p bin
                        curl -fsSL https://download.docker.com/linux/static/stable/x86_64/docker-24.0.9.tgz -o docker.tgz
                        tar -xzf docker.tgz --strip-components=1 -C bin docker/docker
                        rm docker.tgz
                    else
                        echo "Docker ya está disponible en el sistema:"
                    fi
                    docker --version
                '''
            }
        }

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