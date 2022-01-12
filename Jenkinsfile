def label = "mypod-${UUID.randomUUID().toString()}"

//noinspection GrPackage
podTemplate(label: label, cloud: 'kubernetes') {
    node(label) {
        stage('container log') {
            containerLog 'jnlp'
        }
    }
}
