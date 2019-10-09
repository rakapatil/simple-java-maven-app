pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
	 stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
	stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
	stage('s3 upload') {
	   steps { 
		 withAWS(region:'us-east-2',credentials:'s3_jenkinsuser'){


        s3Upload(file:'target/my-app-1.0.jar', bucket:'jenkinsartifactupload', path:'artifacts/')
          }
     }
   }
	
   }
}
    

