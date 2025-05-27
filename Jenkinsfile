node('Ubuntu-app-agent') {  
    def app

    stage('Cloning Git') {
        checkout scm
    }  

    stage('SAST SCAN') {
        build 'snyk_SAST_security'
    }

    stage('Build-and-Tag') {
        app = docker.build("kuber1993/snake")
    }

    stage('Post-to-dockerhub') {
        docker.withRegistry('https://registry.hub.docker.com', 'training_creds') {
            app.push("latest")
        }
    }

    stage('Pull-image-server') {
        sh "docker-compose down"
        sh "docker-compose up -d"
    }

    stage('DAST SCAN') {
        build 'arachni-dast'
    }
}
