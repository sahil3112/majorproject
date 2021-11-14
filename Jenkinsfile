node ('Application Server'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    stage('SECURITY-SAST-SCAN-SNYK') {
        build 'SECURITY-SNYK-SAST'
    }
    stage('SECURITY-SAST-SCAN-SONAR') {
        build 'Sonar-Qube Code Quality'
    }
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
       app = docker.build("sahil3112/game")
    }
    stage('Post-to-dockerhub') {
     docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            app.push("latest")
        			}
         }
    stage('SECURITY-IMAGE-SCAN-AQUA') {
        build 'SECURITY-IMAGE-SCANNER-AQUA'
    }
    
    stage('SECURITY-IMAGE-SCAN-ANCHOR') {
        build 'SECURITY-ANCHOR-IMAGE-SCANNER'
    }
    stage('Pull-image-server') {
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
    stage('SECURITY-DAST-SCAN-ZAP') {
        build 'SECURITY-ZAP-DAST'
    }
    
    stage('SECURITY-DAST-SCAN-ARACHNI') {
        build 'SECURITY-ARACHNI-DAST'
    }
    
}
