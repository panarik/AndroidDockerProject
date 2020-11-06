pipeline {

    agent any
    stages {

        stage("Build Java 7") {
            agent {docker 'openjdk:7'}
            steps {
                sh 'java -version'
            }
        }

        stage("Build Java 8") {
            agent {docker 'openjdk:8'}
            steps {
                sh 'java -version'
            }
        }

    }

}


