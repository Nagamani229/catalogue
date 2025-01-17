pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
   environment { 
        packageVersion = 'sh(script: "node -e \"console.log(require('./package.json').version)\"", returnStdout: true).trim()'
        nexusURL = '172.31.93.240'
    }
    options {
        ansiColor('xterm')
        // timeout(time: 1, unit: 'HOURS')
        // disableConcurrentBuilds()
    }
    
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    // build
    stages {
        stage('Get the version') {
            steps {
                script {
                    def packageVersion = readJSON(file: 'package.json').version
                    packageVersion = packageJson.version
                    echo "application version: $packageversion"
                }
            }
        }
        stage ('install dependencies') {
            steps {
                sh """
                  npm install
                """
            }
        }
        stage ('Build') {
            steps {
                sh """
                  ls -la
                  zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                  ls -ltr
                """
            }
        }
        stage ('publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupID: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsID: 'nexus-auth',
                    artifacts: [
                        [artifactID: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]

                )

            }
        }
        stage('Test') {
         steps {
          script {
            echo "Running tests..."
            // Add your testing commands here, e.g., `npm test`
        }
    }
}

       stage('Deploy') {
        steps {
         script {
            echo "Deploying application..."
            // Add your deployment commands here, e.g., `npm run deploy`
        }
    }
}

        stage('check params') {
            steps{
                sh """
                    echo "Hello ${params.PERSON}"

                    echo "Biography: ${params.BIOGRAPHY}"

                    echo "Toggle: ${params.TOGGLE}"

                    echo "Choice: ${params.CHOICE}"

                    echo "Password: ${params.PASSWORD}"
                """
            }
        }
    }
    // post build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}
  
 
