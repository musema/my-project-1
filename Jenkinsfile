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
    options{
        timeout(time:5,unit:'HOURS')
    }
    stages {
        stage('Prepare'){
            parallel{
                stage('Code-Checkout'){
                    steps{
                       codeCheckout()
                    }
                }
                stage('Prepare-Workspace'){
                    steps{
                        prepareWorkspace()
                    }
                }
            }
        }
        stage("Package") {
            steps {
                package()
            }
        }
        stage("Test"){
            steps{
                runTest()
            }
        }
        stage("Upload to Repository"){
            steps{
                uploadToRepository();
            }
        }
        stage("Decide-Deployment"){
            agent none
            steps{
                script{
                    env.DEPLOY_TO_DEV= input message: 'User input required',
							parameters: [choice(name: 'Do you want to deploy to DEV?', choices: 'no\nyes', description: 'Choose "yes" if you want to deploy the DEV server')]
                }
            }
        }
        stage('Deploy To Dev'){
            when{
                environment name:DEPLOY_TO_DEV, value:'yes'
            }
            steps{
                deploy("DEV")
            }
        }
        stage('Archive'){
            steps{
                archiveArtifacts()
            }
        }
    }
    post {
        always {
            finalizeWorkflow()

        }
            success {
                successMessage()
                 }
        failure {
                failureMessage()
            }
    }
}

//define groovy functions here
def codeCheckout(){
    echo "Checkout the source code from repository"
    echo "The current branch is:${branch}"
}
def prepareWorkspace(){
    echo 'Check here if everything is ready to make a build'

}
def checkoutCode(){

}
def package(){
    sh 'mvn clean install'
    //sh 'mvn clean install -Dmaven.test.failure.ignore=true'
}
def runTest(){
    mvn test
}
def uploadToRepository(){
    echo "We are publishing artifacts to Artifactory:uploadToRepository"
    echo "I need maven:${M2_HOME}"
}
def archiveArtifacts(){
    archive '*/target/**/*'
    junit '*/target/surefire-reports/*.xml'

}
def deploy(deployTo){
    echo "We are deploying to : ${deployTo}"
    

}
def successMessage(){
    echo "Sending SUCCESS email to ${DEVELOPERS_EMAIL}"
    mail to:"${DEVELOPERS_EMAIL}", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build passed."
       

}
def failureMessage(){
    echo "Sending Failure email to ${DEVELOPERS_EMAIL}"
    mail to:"${DEVELOPERS_EMAIL}",subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Hello, the build is failed. Please take immediate action."
}
def finalizeWorkflow(){
    echo "Cleaning up the workspace"
}

