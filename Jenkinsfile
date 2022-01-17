def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",containers: [
    containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d')
  ]) {
  node(label) {
		

		

		stage('Build') {
			git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
				container('maven') {
					sh 'mvn -B -ntp clean package -DskipTests'
				      }
		}

		
    }

}

