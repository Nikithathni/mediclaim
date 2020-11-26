pipeline {
   agent any
	stages {
      stage('Git Checkout') {
         steps {
            git 'https://github.com/Nikithathni/mediclaim.git'
		}
	}
		stage('Approve Build')  {
	 steps {
		input "Deploy to prod?"
			}
		}
			      
		stage('Build') {
         steps {
	    sh '/opt/apache-maven-3.6.3/bin/mvn clean package -Dmaven.test.skip=true'
	         }
	}
        stage ('Execute sonarqube report') {
          steps {
		  sh '/opt/apache-maven-3.6.3/bin/mvn sonar:sonar -Dmaven.test.skip=true'
                 }
	 }
	//stage("Quality Gate") {
          //  steps {
            //  timeout(time: 2, unit: 'MINUTES') {
		//     /* def qGate = waitForQualityGate()
		      
		  //    if(qGate.status != 'OK'){
			//      error "Pripe line status is :{$qGate.status}"
		      // } 
		      // */
               // waitForQualityGate abortPipeline: true
		
            //  }
           // }
         // }
	stage ('Deploy') {
		steps {
			sh '/opt/apache-maven-3.6.3/bin/mvn clean deploy -Dmaven.test.skip=true'
		}
	}
	stage ('Release') {
		steps {
			sh 'export JENKINS_NODE_COOKIE=dontkillme ;nohup java -jar $WORKSPACE/target/*.jar &'
		}
	}
	stage ('DB Migration') {
		steps {
			sh '/opt/bin/mvn clean flyway:migrate'
		}
	}
}
	post {
        always {
            emailext body: "${currentBuild.currentResult}: Project Name : ${env.JOB_NAME} Build ID : ${env.BUILD_NUMBER}\n\n Approval Link :  ${env.BUILD_URL}", recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
    }

}
