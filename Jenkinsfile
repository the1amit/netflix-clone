node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
       app = docker.build("the1amit/netflix-clone")
    }


    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push('latest')hh
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
