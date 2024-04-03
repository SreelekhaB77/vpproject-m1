pipeline {
    
	agent any
	
	tools {
        maven "maven"
	jdk "java11"
    }
	
    environment {
        SNAP_REPO='vpproject-snapshot'
	NEXUS_USER = "admin"
        NEXUS_PASS = "sree"
        NEXUSIP= "172.31.88.187"
        RELEASE_REPO = "vpproject-release"
	NEXUS_GRP_REPO= "vpproject-group"
	CENTRAL_REPO='vpproject-mcentral'
	NEXUSPORT='8081'
        NEXUS_LOGIN='nexuscredentials'
        
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            
        }

	

	
		
        
        

        

    }


}
