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

    stage('Nexus deploy'){ 	
       	steps{           	        	
              nexusPublisher nexusInstanceId: 'localnexus', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/jenkins_home/workspace/'+ env.JOB_NAME +'/test.mtar']], mavenCoordinate: [artifactId: 'jenkins-mtar', groupId: 'org.jenkins-CF.main', packaging: '.mtar', version: '1.' + env.BUILD_NUMBER]]]
        		}        		
			}
    stage('deploy'){
        steps{
		    cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
		}
     }
  }
}
