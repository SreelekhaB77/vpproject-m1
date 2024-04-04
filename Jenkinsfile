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
	SONARSERVER='sonarscanner'
	SONARSCANNER='sonarscanner'
        
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            
         post{
            success {
                echo "Now Archiving....!"
		archiveArtifacts artifacts:'**/*.war'
	    			}
	    		}
		}
      	 
	    stage('TEST'){
            steps {
                sh 'mvn -s settings.xml test'
            	}
	    }
   	
	    stage('checkstyle analysis'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            	}
	    }
	    stage('Sonar Analysis') {
          
		  environment {
             scannerHome = tool "${SONARSCANNER}"
          }

          steps {
            withSonarQubeEnv("${SONARSCANNER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
	  }
	    }
	    stage('Quality Gate'){
		    steps{
			timeout(time: 1,unit:'HOURS'){

				waitForQualityGate abortPipeline: true
				
			}

			    
		    }

	    }
	    stage('Upload Artifact'){
            steps {
                    nexusArtifactUploader(
        		nexusVersion: 'nexus3',
       			 protocol: 'http',
        		nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
       			 groupId: 'QA',
       			 version: "${env.BUILD_ID}-${env.BUILDTIMESTAMP}",
        		repository: "${RELEASE_REPO}",
        		credentialsId: "${NEXUS_LOGIN}",
        	artifacts: [
           		 [artifactId: 'vproapp',
            			 classifier: '',
            			 file: 'target/vprofile-v2.war',
             			type: 'war']
       			 ]
    		 )
            	}
	    }
    
    
    
    
    
    
    
    
    
    }
            
post {
        success {
            emailext subject: "SUCCESS: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                      body: "The job ${env.JOB_NAME} succeeded. Build URL: ${env.BUILD_URL}",
                      to: 'sreelekha789sl@gmail.com'
        }
        failure {
            emailext subject: "FAILURE: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                      body: "The job ${env.JOB_NAME} failed. Build URL: ${env.BUILD_URL}",
                      to: 'sreelekha789sl@gmail.com'
        }
    }
}

	    

	

	
		
        
        

        

    



