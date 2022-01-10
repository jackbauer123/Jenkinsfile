node('jenkins-slave') {
    stage('Checkout') {
        checkout scm
    }
    stage('Build'){
        container('nginx') {
            // This is where we build our code.
        }
    }
}
