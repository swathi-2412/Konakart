pipeline {
    agent any
    tools { 
        maven 'apache-maven-3.3.9' 
        jdk 'jdk1.8.0_172' 
    }
    stages {
        stage ('Initialize') {
            steps {
                
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            
            }
        }
        
       
       stage('Code Checkout') {
            steps { 
                
               git credentialsId: '0323bc7d-f388-4b89-b1c6-c767313f6a95', url: 'http://192.168.175.220/root/Ekart.git'
            }
        }
    
        

        stage ('Build') {
            steps {
                bat 'mvn -Dmaven.test.failure.ignore=true install' 
            }
			post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
            
        }
        
      stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    // Optionally use a Maven environment you've configured already
                   
                        bat 'mvn sonar:sonar'
                    }
                }
            }
        stage ('Artifacts Upload to Archiva') {
            steps {
               // echo "Deploying"
              
               //bat 'echo %CD%'  
               
            //bat 'del D:\\apache-tomcat-7.0.92-windows-x64\\apache-tomcat-7.0.92\\webapps\\konakart.war'
			//bat 'del D:\\apache-tomcat-7.0.92-windows-x64\\apache-tomcat-7.0.92\\webapps\\konakart'
		   // sleep 20
		   bat 'mvn deploy'
            }
        }
	    
	   
        stage ('Deploy To Server') {
            steps {
                echo "Deploying"
              
             bat 'del D:\\apache-tomcat-7.0.92-windows-x64\\apache-tomcat-7.0.92\\webapps\\konakart.war'
             sleep 20
             bat '''cd target
            copy konakart.war D:\\apache-tomcat-7.0.92-windows-x64\\apache-tomcat-7.0.92\\webapps
            '''
            sleep 20
            }
        }
		
		stage ('Functional test') {
            steps {
                echo "Test Started"
              
               //bat 'echo %CD%'  
               
             bat '''d:
             
            cd D:\\EKART\\
            Run.bat
            
            '''
           
            }
        }
        
}
    }

