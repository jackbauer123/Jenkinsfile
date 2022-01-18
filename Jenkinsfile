def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')]
	    //,
    //volumes: [hostPathVolume(hostPath: '/tmp/', mountPath: '/tmp')]
	   ) {
  node(label) {
		

		stage('SCM') {
			//dir('/tmp'){
				git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
			//}
		}

		stage('Build') {
				container('maven') {
					sh 'mvn -B -ntp clean package -DskipTests'
				}
			
		}
	  	
		  stage('image') {
			  //dir('/tmp'){
				  container('docker'){
					docker.withRegistry('https://harbor.yuanzhibin.com', 'harbor-jack') {
									docker.build('oboe-cli').push('t1')
							} 
				  }
			 // }
		  }

		
    }

}

