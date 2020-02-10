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
