pipeline {
    agent any
 
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Akshaya0610/Demo.git'
            }
        }
	stage('BUILD') {
            steps {
                sh 'mvn clean'
		sh 'mvn install'
            }
        }
        stage('Build Docker Image') {

            steps {
                script {
                    app = docker.build("Akshaya0610/oct")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker Image') {

            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
      stage("Deploy To Kuberates Cluster"){
	    steps {
                     sh 'kubectl apply -f test.yml'
               }
	  }
       
    }
}
