def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label, cloud: 'kubernetes') {
    node(label) {
        stage('Run shell') {
            sh 'sleep 130s'
            sh 'echo hello world.'
        }
    }
}
