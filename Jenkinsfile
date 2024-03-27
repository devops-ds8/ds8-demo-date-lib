pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                sh './gradlew clean test'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }
        /*stage('Checkout') { //To check rsa key is correct
            steps {
                git(
                    url: 'git@github.com:devops-ds8/ds8-demo-date-lib.git',
                    credentialsId: 'ssh-key-ds8',
                    branch: 'main'
                )
            }
        }*/
        stage('Show Tags') {
            when {
              branch 'main'
            }
            steps {
               
                    sh '''
                ./gradlew showAllVersionTags
            '''
                
            }
        }
        stage('Release') {
            when {
              branch 'main'
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-ds8', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                ./gradlew release final -Prelease.scope=patch
            '''
                }
            }
        }
        stage('Publish') {
            when {
              branch 'main'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'devops-ds8', usernameVariable: 'USERNAME', passwordVariable: 'TOKEN')]) {
                    sh '''
                        ./gradlew publish -Pgithub_username=$USERNAME -Pgithub_token=$TOKEN
                    '''
                }
            }
        }

    }
}
