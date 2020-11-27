pipeline {
   agent any

//Configuring Pipleline Stages 
	stages {

	//Downloading the source code from github
      stage('Git Clone') {
         steps {
            //git branch: <branch_name> url: <github_url>
		git 'https://github.com/Nikithathni/mediclaim.git'

	  }
	}

	//Getting Approval From Slack for build trigger
	//Run sonar cube scan on the source code before building 

	//Building the application using maven
			      
		stage('Build') {
         steps {
	    sh 'mvn clean package -Dmaven.test.skip=true'
	         }
	}

	//Generate Sonar cube reports 
        stage ('Execute sonarqube report') {
          steps {
		  sh '/opt/apache-maven-3.6.3/bin/mvn sonar:sonar -Dmaven.test.skip=true'
                 }
	 }

	//Saving the built jar inside the nexus repostiory 	
	stage ('Deploy') {
		steps {
			sh 'mvn deploy -Dmaven.test.skip=true'
		}
	}

  }
}
