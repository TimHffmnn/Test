@Library('piper-lib-os') _
pipeline {
  agent any
    stage('prepare') {
        checkout scm
        setupCommonPipelineEnvironment script:this
    }
    stage('build'){
        steps{
	        mtaBuild script: this, mtabBuildTool: ‘cloudMbt’
	    }
    }
    stage('deploy'){
        steps{
		    cloudFoundryDeploy script:this, deployTool:'mtaDeployPlugin', verbose: true
		}
    }

}
