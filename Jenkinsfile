pipeline {

    agent {
        kubernetes {
             yaml '''
apiVersion: v1
kind: Pod
spec:
    containers:
    - name: golang
      image: golang:1.8.0
      command:
        - cat
      tty: true
    - name: python
      image: python:2.7
      command: 
        - cat
      tty: true
    - name: nodejs
      image: node:lts
      command: 
        - cat
      tty: true
    - name: ruby
      image: ruby:2.7.1
      command: 
        - cat
      tty: true
    - name: java
      image: openjdk:11.0.6
      command: 
        - cat
      tty: true
'''
        }
    }

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/r6min/servers'
            }
        }

        stage('Run Servers') {
            parallel {
                stage('Go') {
                    steps {
                        container('golang') {
                            sh '''
                                cd go
                                timeout 10 go run server.go || [ $? -eq 124 ]  
                            '''
                        }
                    }
                }

                stage('Java') {
                    steps {
                        container('java') {
                            sh '''
                                cd java
                                timeout 10 java Server.java || [ $? -eq 124 ]  
                            '''
                        }
                    }
                }

                stage('Node.js') {
                    steps {
                        container('nodejs') {
                            sh '''
                                cd node.js
                                timeout 10 node server.js || [ $? -eq 124 ]  
                            '''
                        }
                    }
                }
            
                stage('Python') {
                    steps {
                        container('python') {
                            sh '''
                                cd python
                                timeout 10 python server.py || [ $? -eq 124 ]  
                            '''
                        }
                    }    
                }

                stage('Ruby') {
                    steps {
                        container('ruby') {
                            sh '''
                                cd ruby
                                timeout 10 ruby server.rb || [ $? -eq 124 ]  
                            '''
                        }
                    }    
                }
            }    
        }
    }
}
