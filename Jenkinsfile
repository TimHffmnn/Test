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
    stage('deploy'){
        steps{
		    cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
		}
     }
  }
}
