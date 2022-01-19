def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')]
	    ,
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
	   ) {
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
	  
	 
		 
	  	
			 stage('build storage image') {
			  //dir('/tmp'){
				  container('docker'){
					  storage = docker.build("jackbauer123/storage:${env.BUILD_ID}","./storage")
				  }
			 // }
		  }
	  
	   stage('build account image') {
			  //dir('/tmp'){
				  container('docker'){
					  account = docker.build("jackbauer123/account:${env.BUILD_ID}","./account")
				  }
			 // }
		  }
	  
	   stage('build order image') {
			  //dir('/tmp'){
				  container('docker'){
					  order = docker.build("jackbauer123/order:${env.BUILD_ID}","./order")
				  }
			 // }
		  }
	  
	  
	   stage('build logic image') {
			  //dir('/tmp'){
				  container('docker'){
					  logic = docker.build("jackbauer123/logic:${env.BUILD_ID}","./logic")
				  }
			 // }
		  }
	  
	  
	  
		  stage('Push image') {
			  container('docker'){
				docker.withRegistry('', 'hubdocker') {
							storage.push("${env.BUILD_NUMBER}")
							storage.push("latest")
					
					account.push("${env.BUILD_NUMBER}")
							account.push("latest")
					
					order.push("${env.BUILD_NUMBER}")
							order.push("latest")
					
					logic.push("${env.BUILD_NUMBER}")
							logic.push("latest")
				} 

			  }	  
		 
			 
		 
	  
	  }
	 
		  
	  
	  	stage('deploy'){
	  
			kubernetesDeploy(kubeconfigId: "'"
					apiVersion: v1
					clusters:
					- cluster:
					    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeU1ERXdOVEF6TkRZek9Gb1hEVE15TURFd016QXpORFl6T0Zvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTFYrCkZuYXUrVCszclFSRVpuNzRONEZTaGE2YUpMWFRibzNwZUd3c25lYnMwMHNYZkM1TFo1RU1XY1ZmUXhpMVZ2enAKeFJTTmk0QnpwejhrZzNITGx5OTZWRU4zZDR1TjR2Vmd2ckRaUGoyby9BQWM0anJlRUovTUVrQnpDNUlYQ3lGdgpvcWRiREdWNktzR2FjR1NlVHMvdWNTNndhZGVIUVM4Ujlla081Q3VWa01hcXllb3BIRk9SNU1Yc3NrcWNZcXJyCklLMTVHTUhydExYb25iNjA3S25oYkk0Q0RCRDhNZ3hkOHdyamMvVHRjQ2pPRGxDNTV5VUthQnJYaFBpcEZGaWoKZkd6R1MwaDJXWnFNNk9sNFJ0TEkyMUxLY3pON0JqNHJkNVU5eFJNWHRUbFJTSENyVGhLa29Sb0dkSlRzMjJNMQo3S0RTYy9WRC9YbVBvVkVzWW1VQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZHckFXL2pNWHFkSmdKZXBJaS9JcnE3V3NOajZNQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBQjFGemVrTXoyM1c1Tld5bzZPVQprUkVqOWZxcG83UjE1emw5RjVVdEFXd0hDRmF5aDNTaFJsZjQvVFM1Z2p1VmUxOEVWOGtxSVNPLzVYODhwUDRlCkV0KzQwV2R6ZllpQW1MRFI5SlIzdzNxWUtBTWJyZUVrZ3hvQkdUZUxzRXA4c2h1MWZGZWxVSWZYbUt0Ty9ndE8Kdk9wdXo0c3djLy8reFExOUFxcTBFQkwxam9HZmN5SHpEV29NbEdVQW50UWk5OTVibUU3MU1ja09xVWYvNVJrSQo5OFNvdWpmMzRVLzZ1bDBQMWhmaGRsV3FUNm1xSjd0djhwUnFPSTFZQ01pWXNVbEhZRlBZeWpHbytoa3NTSjhKCml5UHUvVm9GZlpLYW5GV3J5VG9VMUZXTHYvNVErYk1UNFNSRWkySTREYnNLZWp3QTlZNWs4ZjJ4dk5MdU5GbnoKV0ZzPQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
					    server: https://10.168.1.199:6443
					  name: kubernetes
					contexts:
					- context:
					    cluster: kubernetes
					    user: kubernetes-admin
					  name: kubernetes-admin@kubernetes
					current-context: kubernetes-admin@kubernetes
					kind: Config
					preferences: {}
					users:
					- name: kubernetes-admin
					  user:
					    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURJVENDQWdtZ0F3SUJBZ0lJYmkyWUVGUXh6d0l3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TWpBeE1EVXdNelEyTXpoYUZ3MHlNekF4TURVd016UTJOREphTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXBpUC9kbENuUFpOMU1uN1kKeng0Tlh1bGxDMmQreC81QXkvZU52Um9rQ3FITDQvT3RvWVVXOVhSbW1OZFBhYkg2R2dLOUE0V0NiNm5PT2N2Ngo3YU1qbis0MGU3eG5YK3hnWEJSNm5wWjdpL2xONTZROWcxL3hFMnlkdGp5TlFmNjNZbHVLWUpNc3ZpallTRXNtCklidCtQb3hqZ2V2ajhJb0V0Q2Rsd1V6NG1vd25IbThZRlVoK1UzVDY3ZDNENCtQeTkwWTZJc1M3Z0hEOG9tSjUKUXhtQ0ordHBYRHR4L1MvaHBPdjRYYmk4ZTVOT0VmUkRFa292WlRlRVg3TUxRQjMvcExuMFN3L2FjdW5pRG9lNApTQmR6N3VnK0dQSUQrcTRGNFdiekV2Nmtrb0VtQTBSK2NzSmJsUm9ZT2M0R2J5VHRpOG5xL1dUQjlPZHFSVnBlClJrRTlwd0lEQVFBQm8xWXdWREFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RBWURWUjBUQVFIL0JBSXdBREFmQmdOVkhTTUVHREFXZ0JScXdGdjR6RjZuU1lDWHFTSXZ5SzZ1MXJEWQorakFOQmdrcWhraUc5dzBCQVFzRkFBT0NBUUVBbHh2RWtVeE54U2xNRUM5MFZGT2dpR0tibE9kaUJQTFo3SVZkCndJVHJVRktzd2gyM0w0QjUrZE1ONEhPeUhzUHYwNXI4VjdZZkZCeHNyN2VlZEVESE5OcWpPOWFNVDd1VFJ6ajUKYWlFT21Zb0trMUk3enRESm4zdlVEdHEwZ0ZBUm96cmNwMnZtU095MTJSa3hNSzlQSzNLVUxsbVROVlZxYlowWAp5SncrR2w3cEJ5dHplQ2t6T3MyWlhPUUJ3M1JRTnBYQTNkZkNuMnlMNy9YMEtIZzBxV2x0d2E1d1lqNmpxNUlDCmNKNW02MkQzb3Z2d3UxRm1LR05Ba2h1YzdqdjNKTmxwZ3AxR3o0aTI1T1YxeTV2b2tnMjhtMUxqd2IrYWRVSXEKN2I0UmRzQ2syVDFFa0JXWklkb1BVK1hDdVIwN3UvMHpjcTNnOVdxTHNTZ3B5SVBBTnc9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
					    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBcGlQL2RsQ25QWk4xTW43WXp4NE5YdWxsQzJkK3gvNUF5L2VOdlJva0NxSEw0L090Cm9ZVVc5WFJtbU5kUGFiSDZHZ0s5QTRXQ2I2bk9PY3Y2N2FNam4rNDBlN3huWCt4Z1hCUjZucFo3aS9sTjU2UTkKZzEveEUyeWR0anlOUWY2M1lsdUtZSk1zdmlqWVNFc21JYnQrUG94amdldmo4SW9FdENkbHdVejRtb3duSG04WQpGVWgrVTNUNjdkM0Q0K1B5OTBZNklzUzdnSEQ4b21KNVF4bUNKK3RwWER0eC9TL2hwT3Y0WGJpOGU1Tk9FZlJECkVrb3ZaVGVFWDdNTFFCMy9wTG4wU3cvYWN1bmlEb2U0U0Jkejd1ZytHUElEK3E0RjRXYnpFdjZra29FbUEwUisKY3NKYmxSb1lPYzRHYnlUdGk4bnEvV1RCOU9kcVJWcGVSa0U5cHdJREFRQUJBb0lCQUh1RGM1NHdJOFVoWlJXZgpPK3Z1dVo0QUFjRFN0bXhVVnpQTDNMSGpSendvUVA0ODRLNmQxUTJ4OWJ4WEJaRGNZY1VJbUNDUUZ3S1F5T0lyCkJXZTV5dmhSRE8rWWgzbkdyM1NGUFF1OWNDZ0Z0YWNxY1JqRU1PTng4bVhTNm1sUHhtSDNFQTd3RzJsYjBEOGUKcjBDUXRUQW5DcXRDQlhtRUFpODB3dTlNRzk0NGRNeFN4SkRzemNxSjdZclJJQVpHN3BJUDROU2YrUEw3TUNQZwpRS3ZpRDRRbWtSbnN3K1M3dlh5d3ZKaUo5OTdZdE9yQm9QY0lxOEE4K1lPMUd6RC90TXJxOXZoeDZvaHgrSGw0Cm1qaGVaSW9iT2ZobUw1ZzZPZ09hNzdkSVIwY2ltRjRERVF5L3ZHNWljOHBOeU5KcEhmSVJCNjJzMFRDaGRXck0KRmVZdVBuRUNnWUVBeGRlU01zSVVnZFREUUdQNmVzaXFMYnpYeHZ5TmhzTVJOYUhmZWVZWjE1bWlhclZtbmRsZAp3VUUzOTFSQUdaMGxRaGw3MUtaTnJjOXdWemw2Q2VYTCtJRDZRRm4wam5qRTk4ZG9xNkpQNDRxU2J1dG5oRVRmClFKL3BOajM5ZUE3QXdpOXFYRko0QmdMYi9OL05OelgxZCsyU2w5Rmo0S1plK3pvWFJnejJUZDBDZ1lFQTF2ckQKQ2p1YXBCeExEeWtVUUZCVXd6ZEVHRys0RmZITkFpT0k2VFhmRDZzZXk4NzVsQVRpcW5oZlZGcGRESW9BTU92dworaGxCQ2dZaDFkbk5CbEtrK1M1U1h6d3ZtQ0dNZmx1QWw0TjhuS2NoQzVLaUJ4UnRsZkp1RXFlV0lWNDhpb204CjV6OVcwcUlyUTVYRWpzaWJhWlJYNmRSNWhsamtHWnVVVFNvcmkxTUNnWUVBa085UzZDSnNPWXl2bVYxazQweGIKOTNQUHM3UFN6blhiQnFwV2VBdk14TGlGVnAwYjF1bWxtR3o1M2hQM2ZZdzAvazZDL0E3MCt5N3Jnc3JWajZpcwpHNW9KT3Rscm9tL3hCQUF1dXdZR2RwQk9wRG1LTlVqck1JRzFySW9QUlVPeGpOQ0ZuNnEreU5DUmJwaFowMmVSCk0xRjB3Z21nbkxQbEF6RFVXZm9tK25VQ2dZQkZTZytCQmRNQzRBZWxQRVZGc1Y2UWlRaU9vN3QrZnkvblo1S2kKTC9YVU5BQTZDbHpRdzM1WEdYTUlXaE94amUwZjEzd1U3L3pSZ1VaNGlibVdOeDdySFczNU9nblJDOGNmbHRocwpmVG0xdC94am9ZQk5yZHpnUG9JUnl5Z05XelZDSmNEWCs4YzlIbjI1UzlWTmZBVHpVNWUrU3ZoY1A5eE5FS01NCjkvR085d0tCZ1FDZ1VoaHluMjFOSUpzcmUwMVlCbWtLOEVaRzVXMlc0Ni9WYTlENndteENlYTRQVzRzdGRMWTYKK0lrMXhzd2VmaGhDYndJN1ZYOUJkWGthNlpKekdOa1BhVlgraU5adktWMUJPYXVOZFhpUlNmekpsdnA0bGRFcQpBNWpyT2s4VEkrejh4VzVwaVJBZCsrNUYyQWoxQnphOEZ6R3RHbFVZK1FkN2tPQytUOEprZ0E9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo=
					 
					 "'",               // REQUIRED

					 configs: './account/account.yaml,./storage/storage.yaml,./order/order.yaml,./logic/logic.yaml', // REQUIRED
					 enableConfigSubstitution: false
					 //,

					 //secretNamespace: '<secret-namespace>',
					 //secretName: '<secret-name>',
					 //dockerCredentials: [
					//	[credentialsId: '<credentials-id-for-docker-hub>'],
					//	[credentialsId: '<credentials-id-for-other-private-registry>', url: '<registry-url>'],
					 //]
			)
	  
	  	}	

		
    }

}

