pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        // Force Java 17 to match local compatible version and fix Lombok issues
        JAVA_HOME = '/Users/mahdi/Library/Java/JavaVirtualMachines/ms-17.0.17/Contents/Home'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {

        stage('Cloner le dépôt') {
            steps {
                echo 'Clonage du dépôt GitHub...'
                git branch: 'main', url: 'https://github.com/benmekkielmahdi/Jenkins'
            }
        }

        stage('Build and SonarQube Analysis') {
            parallel {

                stage('Car Service') {
                    stages {

                        stage('Build Car Service') {
                            steps {
                                dir('car') {
                                    echo 'Compilation et génération du service Car...'
                                    sh 'mvn clean install -DskipTests'
                                }
                            }
                        }

                        stage('SonarQube Analysis Car Service') {
                            steps {
                                dir('car') {
                                    script {
                                        def mvn = tool 'maven'
                                        withSonarQubeEnv('SonarQube-Car') {
                                            sh """
                                                ${mvn}/bin/mvn clean verify \
                                                sonar:sonar \
                                                -Dsonar.projectKey=car \
                                                -Dsonar.projectName=car \
                                                -DskipTests
                                            """
                                        }
                                    }
                                }
                            }
                        }
                    }
                }

                stage('Client Service') {
                    stages {

                        stage('Build Client Service') {
                            steps {
                                dir('client') {
                                    echo 'Compilation et génération du service Client...'
                                    sh 'mvn clean install -DskipTests'
                                }
                            }
                        }

                        stage('SonarQube Analysis Client Service') {
                            steps {
                                dir('client') {
                                    script {
                                        def mvn = tool 'maven'
                                        withSonarQubeEnv('SonarQube-Client') {
                                            sh """
                                                ${mvn}/bin/mvn clean verify \
                                                sonar:sonar \
                                                -Dsonar.projectKey=client \
                                                -Dsonar.projectName=client \
                                                -DskipTests
                                            """
                                        }
                                    }
                                }
                            }
                        }
                    }
                }

                stage('Gateway Service') {
                    steps {
                        dir('gateway') {
                            echo 'Compilation et génération du service Gateway...'
                            sh 'mvn clean install -DskipTests'
                        }
                    }
                }

                stage('Eureka Server') {
                    steps {
                        dir('server_eureka') {
                            echo 'Compilation et génération du serveur Eureka...'
                            sh 'mvn clean install -DskipTests'
                        }
                    }
                }
            }
        }

        stage('Docker Compose') {
            steps {
                dir('deploy') {
                    echo 'Création et déploiement des conteneurs Docker...'
                    sh 'docker-compose up -d --build'
                }
            }
        }
    }
}
