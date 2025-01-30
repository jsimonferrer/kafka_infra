pipeline {
    agent any

    stages {
        stage('Levantar servicios con Docker Compose') {
            steps {
                script {
                    // Ejecutar docker-compose para levantar los servicios
                    sh 'docker-compose -f docker-compose.yml up -d'
                }
            }
        }

        stage('Verificar servicios') {
            steps {
                script {
                    // Esperar a que Zookeeper esté listo
                    sh '''
                        while ! docker exec zookeeper_jsf zkServer.sh status; do
                            sleep 5
                            echo "Esperando a que Zookeeper esté listo..."
                        done
                    '''

                    // Esperar a que Kafka esté listo
                    sh '''
                        while ! docker exec kafka_broker_jsf kafka-topics.sh --list --bootstrap-server localhost:9092; do
                            sleep 5
                            echo "Esperando a que Kafka esté listo..."
                        done
                    '''

                    // Verificar que Kafka UI esté en ejecución
                    sh 'curl -I http://localhost:9090'
                }
            }
        }

        stage('Ejecutar pruebas') {
            steps {
                script {
                    // Aquí puedes agregar pruebas o acciones adicionales
                    echo "Ejecutando pruebas..."
                }
            }
        }
    }

    post {
        always {
            // Detener y eliminar los contenedores
            sh 'docker-compose -f docker-compose.yml down'
        }
    }
}