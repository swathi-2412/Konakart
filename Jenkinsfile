node {
     
      stage ('Build')
      {
         
             sh "mvn -Dmaven.test.failure.ignore=true clean package" 
          
      }
      stage ('Code Coverage'){
         
              sh "mvn sonar:sonar"
          
      }
      stage ('Upload_Artifact')
      {
         
              rtUpload (
                  serverId: "arti",
			spec:       
			"""{ 
			"files": [            
			{             
			  "pattern": "target/*.war",             
			  "target": "KonakartM_Maven_App/"            
			}         
			]        
		    }""")
          
      }
      stage('Download_Artifact')
        {    
           
		    rtDownload (
			serverId: "arti",  
			spec:
			"""{
			"files": [
			{
			  "pattern": "KonakartM_Maven_App/",
			  "target": "/var/lib/jenkins/workspace/konakart.war"
			}
			]
			}"""
		    )
	    }
	    stage ('Deploy_To_Tomcat')
	    {
	      
	             sh '''cp -R "/var/lib/jenkins/workspace/konakart.war" "/opt/tomcat/apache-tomcat-9.0.30/webapps"
           '''
	        
	    }
}
node {
    
	 stage('Build image') {
        /* This builds the actual image */

       sh sudo docker.build("chiducaff/build_pipeline") 
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
     sh sudo docker.withRegistry('https://registry.hub.docker.com', 'Docker') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
}
