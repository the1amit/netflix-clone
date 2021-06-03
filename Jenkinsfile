node {
    def app

    stage('Clone repository') {
        checkout scm
    }


  //  stage('SonarQube analysis') {
  //     def scannerHome = tool 'SonarScanner 4.0';
  //     withSonarQubeEnv('SonarQubeScanner') {
//	       sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=react -Dsonar.sources=. -Dsonar.host.url=http://vmpl1000.eastus.cloudapp.azure.com:9000"
  //     }
    //}
	
    stage('Build image') {
       app = docker.build("the1amit/netflix-clone")
    }


    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push('latest')
        }
    }
	
    stage('Deployment') {
	
	try{
		sh 'helm install reactjs-app ./netflix-charts/'
	}
	catch(error){
		sh 'helm upgrade reactjs-app ./netflix-charts/'
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
