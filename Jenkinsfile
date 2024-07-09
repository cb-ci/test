#!groovy

/*
podTemplate(
    label: 'mypod',
    containers: [
        containerTemplate(
            name: 'ciutils',
            image: 'alpine/git',
            ttyEnabled: true,
            command: 'cat'
        )
    ]
) {

    node('mypod') {
        stage('Build') {
            container('ciutils') {

                sh 'ls -la'
            }
        }
    }
}
*/

// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
             yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: alpine/git
    command:
    - sleep
    args:
    - infinity
'''
            defaultContainer 'shell'
            retries 2
        }
    }
    stages {
        stage('Main') {
            steps {
                sh "ls -la"                
                sh "git config --global --add safe.directory ${WORKSPACE}"
                echo "removed lines"
                sh "git status"
                sh 'git diff HEAD^^ HEAD -- branches |grep ^-.*'
                echo "added lines"
                sh 'git diff HEAD^^ HEAD -- branches |grep ^+.*'
            }
        }
    }
}

