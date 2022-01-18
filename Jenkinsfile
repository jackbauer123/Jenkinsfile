def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')],
    volumes: [hostPathVolume(hostPath: '/tmp', mountPath: '/tmp')]) {
  node(label) {
		

		

		stage('Build') {
			git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
			container('maven') {
				sh 'mvn -B -ntp clean package -DskipTests'
			}
		}
	  	
		  stage('image') {
			  container('docker'){
				docker.withRegistry('https://harbor.yuanzhibin.com', 'registry-hub-credentials') {
								docker.build('oboe-cli').push('t1')
						} 
			  }
		  }

		
    }

}

