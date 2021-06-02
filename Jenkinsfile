node {
    def app

    stage('Clone repository') {
        checkout scm
    }


    stage('SonarQube analysis') {
        environment {
            scannerHome = tool 'sonarqube-8.9.0.43852'
        }
       
	withSonarQubeEnv('SonarQubeScanner') {
	    sh '''
                  sonar-scanner \
		  	   ${JENKINS_HOME}/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQube_Scanner/bin/sonar-scanner \
			  -Dsonar.projectKey=react \
			  -Dsonar.sources=. \
			  -Dsonar.host.url=http://vmpl1000.eastus.cloudapp.azure.com:9000 \
			  -Dsonar.login=c0f7391189348afef9684202569b17e9e02c7645
                '''
	}
      
    }
	
    stage('Build image') {
       app = docker.build("the1amit/netflix-clone")
    }


    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push('latest')
        }
    }
    stage('Send email Notification') {
	def mailRecipients = "the_amit@live.com"
    	def jobName = currentBuild.fullDisplayName

	emailext (
		body: '''${SCRIPT, template="groovy-html.template"}''',
		mimeType: 'text/html',
		subject: "[Jenkins] ${jobName}",
		to: "${mailRecipients}"
	)
    }
	
    
}
