def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes") {
  node {
		stage('Clone Code') {
				dir('baas-ops') {
						git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
				}
		}

		

		stage('Build') {
				container('maven') {
					sh 'mvn -B -ntp clean package -DskipTests'
				      }
		}

		stage('Image Build And Push') {
				dir('baas-ops/oboe/cli') {
					 docker.withRegistry('https://harbor.yuanzhibin.com:8080', 'registry-hub-credentials') {
								docker.build('oboe-cli').push('t1')
						} 
				}

		}

		stage('Deploy images') {
				try {
						sh 'docker rm -f test'
				} catch(e) {

				}
				docker.image('oboe-cli:t1').run('-p 80:3000 --name test')
		}
    }

}

