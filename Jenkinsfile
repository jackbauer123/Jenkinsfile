node('jenkins-slaves') {
    stage('Checkout') {
        checkout scm
    }
    stage('Build'){
        container('go-agent') {
            // This is where we build our code.
        }
    }
}
