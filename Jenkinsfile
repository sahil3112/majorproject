node ('linux') {  
    //def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }
    stage('SNKY-SAST') {
        sh "cd ${WORKSPACE}"
        sh "snyk code test --json > snyk-sast.json || true"
        sh "snyk-to-html -i snyk-sast.json -o snyk-sast.html"
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '.', reportFiles: 'snyk-sast.html', reportName: 'Snyk SAST Report', reportTitles: 'Snyk SAST Report'])
    }
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        sh "sudo chmod 666 /var/run/docker.sock"
      app = docker.build("sahil3112/major_project")
    }
    stage('Post-to-dockerhub') {
        docker.withRegistry('https://registry.hub.docker.com', 'Docker') {
            app.push("latest")
        			}
         }
    stage('Pull-image-server') {
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }    
}
