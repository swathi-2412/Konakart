pipeline{
	agent any
	 environment {
    registry = "chiducaff/build_pipeline"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
	stages{
      stage ('Build')
      {
	      steps{
             sh "mvn -Dmaven.test.failure.ignore=true clean package" 
          
	      }}
      stage ('Code Coverage'){
	      steps{
         
              sh "mvn sonar:sonar"
          
	      }}
      stage ('Upload_Artifact')
      {
	      steps{
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
          
	      }}
      stage('Download_Artifact')
        {    
		steps{
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
		}}
	    stage ('Deploy_To_Tomcat')
	    {
		    steps{
	             sh '''cp -f "/var/lib/jenkins/workspace/konakart.war" "/opt/tomcat/webapps"
           '''
	        
		    }}
    
	 stage('Build image') {
	 steps{
       
       sh '''sudo docker build -t java:jdk -f Dockerfile . '''
    }}
stage('Push image') {
steps{
    script{
   docker.withRegistry( '', registryCredential ) 
    {
          //dockerImage.push()
        
          sh ''' sudo docker push chiducaff/build_pipeline:jdk'''
}}
}
}}}
