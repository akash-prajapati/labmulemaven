
node {
    
   def mvnHome
   
   def cuser   = params.clouduser 
   def cpwd    = params.cloudpwd
   def prjname = params.prjname 
   
   def env = 'local'
    
   stage('SCM') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/akash-prajapati/labmulemaven.git'
     
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured in the global configuration.
      mvnHome = tool 'MAVEN_HOME'
   }
   
   /* Deploying to the DEV Machine*/
   stage('DEV') {
	  env = 'dev'
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package -P ${env}"
      } else {
            bat(/"${mvnHome}\bin\mvn" clean package deploy -Dcloudhub.username=${cuser} -Dcloudhub.password=${cpwd} -Dcloudhub.domain=${prjname}-${env} -P ${env}/)
      }    
   }
   
    /* Confirmation for Production Deployment. */
   stage("WAIT_FOR_USER_INPUT") {
            def userInput = input(id: 'userInput', message: 'Let\'s promote?', parameters: [
        [$class: 'TextParameterDefinition', defaultValue: 'yes', description: 'Go Ahead', name: 'response']])
        
        
    }
	
   /* Deploying to the SANDBOX Machine*/
   stage('SANDBOX') {
   
	  env = 'sandbox'
	  
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package -P ${env}"
      } else {
          bat(/"${mvnHome}\bin\mvn" clean package deploy -Dcloudhub.username=${cuser} -Dcloudhub.password=${cpwd} -Dcloudhub.domain=${prjname}-${env} -P ${env}/)
      }    
   }        
}
