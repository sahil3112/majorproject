node ('linux') {  
    //def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }
    stage('SNYK-SAST'){
        build 'SNYK-SAST'
    }
    stage('SNYK-SCA'){
        build 'SNYK-SCA'
    }
    stage('DC-SCA') {
        sh "npm install --package-lock"
        build 'Dependency-Check-SCA'
    }
    stage('DT-SCA') {
        build 'Dependency-Track-SCA'
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
    stage('SNYK-Container-Security-Testing') {
        build 'Dependency-Track-SCA'
    }
    stage('Pull-image-server') {
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }    
}
