
node {
   
   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
	credentialsId: 'github', 
	url: 'https://github.com/sainuthalapati/myweb'
   
   }
	
   stage('Mvn Package'){
	   // Build using maven
	   def mvn = tool (name: 'maven3', type: 'maven') + '/bin/mvn'
	   sh "${mvn} clean package"
   }
   
   stage('deploy-dev'){
       def tomcatDevIp = 	
	   def tomcatHome = '/opt/tomcat8/'
	   def webApps = tomcatHome+'webapps/'
	   def tomcatStart = "${tomcatHome}bin/startup.sh"
	   def tomcatStop = "${tomcatHome}bin/shutdown.sh"
	   
	   sshagent (credentials: ['tomcat8']) {
	      sh "scp -o StrictHostKeyChecking=no target/myapp*.war ec2-user@${tomcatDevIp}:${webApps}myapp*.war"
          sh "ssh ec2-user@${tomcatDevIp} ${tomcatStop}"
		  sh "ssh ec2-user@${tomcatDevIp} ${tomcatStart}"
       }
   }
   stage('Email Notification'){
		mail bcc: '', body: '''Hi Team, You build successfully deployed

Thanks,
DevOps Team''', cc: '', from: '', replyTo: '', subject: 'Build Success', to: 'sainuthalapati123@gmail.com'
   
   }
   stage('Slack Notification'){
     slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'nyc', color: 'bad', iconEmoji: '', message: 'Deployed Sucessfully', teamDomain: 'devops', token: 'TKNJ4AFU0/BKQE5G407/rXWkdBxNX9QVxkdHykuejP5I', username: '' 
   }
   stage('creating docker image'){
   sh "docker build -t app:1 ."
   }
   stage('creating container'){
       sh "docker run -d -P app:1"
   }
}
