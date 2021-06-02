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
	    sh '/opt/${scannerHome}/bin/linux-x86-64 -D sonar.sources=./src '
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
