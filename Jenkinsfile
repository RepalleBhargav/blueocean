pipeline {
    agent none
    stages {
        stage('build and test') {
            agent any 
            environment {
                GOCACHE = '/tmp/gocache'
            }
            steps {
                sh 'go build'
                sh 'go test'
                
            }
        }
        stage('deploy') {
            agent any
            environment {
                BUILD_NUMBER_BASE = '0'
                VERSION_MAJOR = '0'
                VERSION_MINOR = '1'
                VERSION_PATCH = "${env.BUILD_NUMBER.toInteger() - BUILD_NUMBER_BASE.toInteger()}"
            
            }
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('', 'dockerhub') {
                        def customImage = docker.build("riyaz1994/example:$VERSION_MAJOR.$VERSION_MINOR.$VERSION_PATCH")
                        customImage.push()
                    }
                }
            }
        }
    }
}
