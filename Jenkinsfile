def label = "mypod-${UUID.randomUUID().toString()}"

//noinspection GrPackage
podTemplate {
    node(label) {
        stage('container log') {
            containerLog 'jnlp'
        }
    }
}
