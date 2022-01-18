def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')],
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {
  node(label) {
		
		environment {
			registry = "YourDockerhubAccount/YourRepository"
			registryCredential = 'dockerhub_id'
			dockerImage = ''
		}
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
	 
		  stage('build image') {
			  //dir('/tmp'){
				  container('docker'){
					  app = docker.build("harbor.yuanzhibin.com/test")
				  }
			 // }
		  }
	  
		  stage('Push image') {
			  container('docker'){
				docker.withRegistry('', 'hubdocker') {
							app.push("${env.BUILD_NUMBER}")
							app.push("latest")
				} 

			  }	  
		 }

		
    }

}

