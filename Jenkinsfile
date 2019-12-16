pipeline {
    agent any
    tools {
        maven 'apache-maven-latest'
        jdk 'openjdk-jdk11-latest'
    }
    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean verify --batch-mode package' 
            }
        }

        stage('Deploy') {
            when { branch 'master' }
            steps {
            	parallel {
            	    p2: {
            	        build job: 'deploy-p2-modelserver', wait: false
            	    }
            	    m2: {
            	        build job: 'deploy-m2-modelserver', wait: false
            	    }
            	}
            }
        }
    }
    post {
        always {
            junit '**/surefire-reports/*.xml'
        }
    }
}
