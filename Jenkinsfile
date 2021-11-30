pipeline {
    agent any
    tools { 
        maven 'Maven' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
      
      stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
      stage ('Check-Git-Secrets') {
		    steps {
	        sh 'rm trufflehog || true'
		sh 'docker run gesellix/trufflehog --json https://github.com/vipertrent/webapp.git > trufflehog'
		sh 'cat trufflehog'
	    }
	    }
        
        stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.110.143.87:/prod/apache-tomcat-9.0.55/webapps/webapp.war'
              }      
           }       
    }
      
    }
}
