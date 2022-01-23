def label_scm = "mypod-${UUID.randomUUID().toString()}"
def label_build_common = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_account = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_storage = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_order = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_logic = "mypod-${UUID.randomUUID().toString()}"

parameters {
	string(name: 'buid_id', defaultValue: "${env.BUILD_NUMBER}", description: 'What should I say?')
  }


podTemplate(label: label_scm,cloud: "kubernetes",containers: [containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8',command: 'sleep', args: '99d')])
{
	node(label_scm) {
		stage('SCM'){
			git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'	
			script {
			    build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
			    if (env.BRANCH_NAME != 'master') {
				build_tag = "${build_tag}-${env.BUILD_NUMBER}"
			    }
			}
	    	}
		
	}	
	node(label_build_common) {
			stage('build common'){
				container('maven') {
					sh 'mvn -B -ntp clean package -DskipTests -f account/pom.xml'
				}
		   	 }


	}



}


	


