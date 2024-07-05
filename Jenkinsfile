#!groovy

// Define the pod template
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
    // Use the Kubernetes pod as the agent
    node('mypod') {
        stage('Build') {
            container('ciutils') {
                // Run  commands
                sh 'ls -la'
            }
        }
    }
}


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
                sh 'git diff HEAD^ HEAD -- branches'
            }
        }
    }
}

