pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        DOCKER_IMAGE_NAME = 'tomcat:latest'
        SAMPLE_WAR_FILE = 'sample.war'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE_NAME)
                }
            }
        }

        stage('Run Docker Compose') {
            steps {
                script {
                    sh "docker-compose -f ${DOCKER_COMPOSE_FILE} up -d"
                }
            }
        }

        stage('Copy sample.war to Tomcat webapps') {
            steps {
                script {
                    docker.cp("${SAMPLE_WAR_FILE}", "${DOCKER_IMAGE_NAME}:/usr/local/tomcat/webapps/")
                }
            }
        }
    }

    post {
        always {
            // Cleanup, if needed
            script {
                sh "docker-compose -f ${DOCKER_COMPOSE_FILE} down"
            }
        }
    }
}

