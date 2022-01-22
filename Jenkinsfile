def label_scm = "mypod-${UUID.randomUUID().toString()}"
def label_build_common = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_account = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_storage = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_order = "mypod-${UUID.randomUUID().toString()}"
def label_bulid_logic = "mypod-${UUID.randomUUID().toString()}"

parameters {
	string(name: 'buid_id', defaultValue: "${env.BUILD_NUMBER}", description: 'What should I say?')
  }

stage('SCM'){
	
	podTemplate(label: label_scm,cloud: "kubernetes")
	{
	    node(label_scm) {
			git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'	
			script {
			    build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
			    if (env.BRANCH_NAME != 'master') {
				build_tag = "${build_tag}-${env.BUILD_NUMBER}"
			    }
			}
	    }
	}
	
	

}

stage('build common'){
	
	podTemplate(label: label_build_common,cloud: "kubernetes",containers: [containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8')]
		   ){
		    node(label_build_common) {
				container('maven') {
					sh 'mvn -B -ntp clean package -DskipTests -f samples-common/pom.xml'
				}
		    }
	}
	
}

stage ('build') {
	parallel (
		"account": {
			podTemplate(label: label_bulid_account,cloud: "kubernetes",containers: [containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8')]){
				    node(label_bulid_account) {
						container('maven') {
							sh 'mvn -B -ntp clean package -DskipTests -f account/pom.xml'
						}
				    }
			}
			
		
		
		},
		"order": {
			podTemplate(label: label_bulid_storage,cloud: "kubernetes",containers: [containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8')]){
				    node(label_bulid_storage) {
						container('maven') {
							sh 'mvn -B -ntp clean package -DskipTests -f storage/pom.xml'
						}
				    }
			}
			
		
		},
		"storage": {
			podTemplate(label: label_bulid_order,cloud: "kubernetes",containers: [containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8')]){
				    node(label_bulid_order) {
						container('maven') {
							sh 'mvn -B -ntp clean package -DskipTests -f order/pom.xml'
						}
				    }
			}
			
		
		},
		"logic": {
			podTemplate(label: label_bulid_logic,cloud: "kubernetes",containers: [containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8')]){
				    node(label_bulid_logic) {
						container('maven') {
							sh 'mvn -B -ntp clean package -DskipTests -f logic/pom.xml'
						}
				    }
			}
			
		
		}
	
	
	) 

}
	


