@Library('piper-lib-os') _
pipeline {
  agent any
  stages{
    stage('prepare') {
        steps{
            checkout scm
            setupCommonPipelineEnvironment script:this
        }
    }
    stage('build'){
        steps{
	         mtaBuild script: this, mtaBuildTool: 'cloudMbt'
             
	    }
    }
    stage('check'){
        steps{   	
    		script{
					git "https://github.com/TimHffmnn/test"
    				def scannerHome = tool 'SonarCloud';
    				withSonarQubeEnv('SonarCloud') { 
    				 sh "${scannerHome}/bin/sonar-scanner"}				    
    				}	   					
			}
    	}
    stage('Nexus deploy'){ 	
       	steps{           	        	
              nexusPublisher nexusInstanceId: 'localnexus', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/jenkins_home/workspace/Doku_Review_master/test.mtar']], mavenCoordinate: [artifactId: 'jenkins-mtar', groupId: 'org.jenkins-CF.main', packaging: '.mtar', version: '1.' + env.BUILD_NUMBER]]]
        		}        		
			}
    stage('TMS'){
        steps{
            tmsUpload script:this, verbose: true, nodeName: 'start', credentialsId: 'TMS_Credential', mtaPath: 'test.mtar', customDescription: 'Job Name: '+ env.JOB_NAME+' Build Nr: '+ env.BUILD_NUMBER
        }
    }

    stage('deploy'){
        steps{
		    cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
		}
     }
  }
}
