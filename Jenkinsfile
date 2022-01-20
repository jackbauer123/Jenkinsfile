def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'jnlp',image: 'hub.sx.com/ops/jenkins-slave')]
	    ,
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
	   ) {
  node(label) {
		
		environment {
			registry = "YourDockerhubAccount/YourRepository"
			registryCredential = 'dockerhub_id'
			dockerImage = ''
        		JOB_NAME = "${JOB_NAME}".replace("-deploy", "")
       			 REGISTRY = "my-docker-registry"
			KUBECONFIG_CREDENTIAL_ID = 'kubeconfig-credentials-id'
  	
		}
	  
	  
	  stage('SCM') {
			//dir('/tmp'){
				git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
			//}
		}
		
	  	stage('deploy account'){
	  
			withKubeConfig([credentialsId: 'kube2', serverUrl: 'https://10.168.1.199:6443']) {
			      sh 'kubectl apply -f account/account.yaml'
			    }
			
			//container('maven') {
			      // 如果不提供 kubeconfigFile，则 kubectl 上下文找不到
			  //    withCredentials([kubeconfigFile(credentialsId: 'kubeconfig-credentials-id', variable: 'KUBECONFIG')]) {
			//	sh 'kubectl apply -f account/account.yaml'
			  //    }
			   // }
			
			//kubernetesDeploy(kubeconfigId: 'kubeconfig-credentials-id',               // REQUIRED

			//		 configs: 'account/account.yaml', // REQUIRED
			//		 enableConfigSubstitution: false
					 //,

					 //secretNamespace: '<secret-namespace>',
					 //secretName: '<secret-name>',
					 //dockerCredentials: [
					//	[credentialsId: '<credentials-id-for-docker-hub>'],
					//	[credentialsId: '<credentials-id-for-other-private-registry>', url: '<registry-url>'],
					 //]
			//)
	  
	  	}
	  
	
	 

		
    }

}

