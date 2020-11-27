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
		//Generate Sonar cube reports 
        stage ('Execute sonarqube report') {
          steps {
		  sh '/opt/apache-maven-3.6.3/bin/mvn sonar:sonar -Dmaven.test.skip=true'
                 }
	 }

	//Getting Approval From Slack for build trigger
		stage('Slack_Aproval_job') {
        // steps {
	  //  slackSend channel:'#jenkinstest',message:'hello slack'
	    //     }
	}
	//Run sonar cube scan on the source code before building 

	//Building the application using maven
			      
		stage('Build') {
         steps {
	    sh '/opt/apache-maven-3.6.3/bin/mvn clean package -Dmaven.test.skip=true'
	         }
	}


	//Saving the built jar inside the nexus repostiory 	
	stage ('Deploy') {
		steps {

			//sh '/opt/apache-maven-3.6.3/bin/mvn deploy -Dmaven.test.skip=true'
			sh '/opt/apache-maven-3.6.3/bin/mvn clean deploy -DskipTests'

			
		}
	}
	// Triggering the ansible 
	//stage ('Deploy_ansible') {
		//steps {
			//sh '/opt/apache-maven-3.6.3/bin/mvn deploy -Dmaven.test.skip=true'
			//sh 'sudo su - ansible'
			//sh 'ansible-playbook ansible2.yml'
		//}
	//}



  }
}
