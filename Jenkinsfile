pipeline {
    agent any
    triggers {
  pollSCM '* * * * *'
}
    tools {
  maven 'M2_HOME'
}
    stages {
        stage('Build war') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                sh 'mvn package'
            }
        }
        stage('Testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerid'){
                        def app = docker.build('femiodedina/ng:${env.BUILD_NUMBER}')
                        app.push()
                        app.push("latest")
                    }
                }
            }
        }
    }
}