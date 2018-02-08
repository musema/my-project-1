pipeline {
    agent any
    
    tools {
        maven "Maven-3.3.9"
        jdk "Java-8"
    }
    environment{
        DEVELOPERS_EMAIL="musema.hassen@gmail.com"
        BRANCH="develop"
    }
    stages {
        stage('Prepare'){
            parallel{
                stage('Code-Checkout'){
                    steps{
                        echo "Checkout the source code from repository"

                    }
                }
                stage('Prepare-Workspace'){
                    steps{
                        echo 'Hello, we are preparing the workspace'
                    }
                }
            }
        }
        stage("Package") {
            steps {
                sh 'echo "path: ${PATH}"'
                sh 'echo "M2_HOME: ${M2_HOME}"'
                sh 'mvn clean install'
                //     sh 'mvn clean install -Dmaven.test.failure.ignore=true'
            }
        }
        stage("Test"){
            steps{
                sh 'mvn test'
            }
        }
        stage("Upload to Repository"){
            steps{
                echo 'We will be upload artifacts to repository here'
            }

        }
        stage('Deploy To Dev'){
            steps{
                echo 'We will be deploying to Development here'
            }
        }
    }
    post {
        always {
            echo "We will be generating reports here"
            echo "Clean up the work space here"
        }
            success {
            echo "Sending SUCCESS email to {DEVELOPERS_EMAIL}"
            mail to:"musema.hassen@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build passed."
        }
        failure {
            echo "Sending Failure email to {DEVELOPERS_EMAIL}"
            mail to:"musema.hassen@gmail.com",subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Hello, the build is failed. Please take immediate action."
        }
    }
}
