pipeline {
    agent any
	environment {
        verCode = UUID.randomUUID().toString()
        registryCredential ='docker'
    }
	tools {
        maven 'maven' 
    }
    stages {
        stage('GIT clone repo and creation of version.html') {
            steps {
			  //get repo
              git 'https://github.com/vishvaja0630/AutomationAssignment.git'
			  
			  //Creating version.html and writing randomUUID to it
		      sh script:'''
		    	touch musicstore/src/main/webapp/version.html
		      '''
		     println verCode
		     writeFile file: "musicstore/src/main/webapp/version.html", text: verCode
            }
        }
	stage('Build maven project'){
		    steps{
			  sh script:'''
			  cd musicstore
			  mvn -Dmaven.test.failure.ignore=true clean package
		      '''
			}
	}
	stage('Docker build and publish'){
		steps{
		    script{
			 dockerImage = docker.build("shivani221/mytomcatimage")
			 docker.withRegistry( '', registryCredential ) {
                         dockerImage.push("$BUILD_NUMBER")
                         dockerImage.push('latest')
			 }
			}
		}
	}    
	    
	stage('Running the container') {
		steps{
	         sh 'docker run -d --name mytomcatimage -p 9090:8080 shivani221/mytomcatimage:latest'
	        }
	 }
		
    }  
}
