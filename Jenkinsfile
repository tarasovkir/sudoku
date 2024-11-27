node('agent1') {
    def app

    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }

    stage('SAST') {
        build 'SCA-SAST-SONARQUBE'
    }

    stage('Build-and-Tag') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("greygoo42/sudoku")
    }

    stage('Post-to-dockerhub') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_creds') {
            app.push("latest")
        }
    }

    stage('Pull-image-server') {
        sh "docker-compose down"
        sh "docker-compose up -d"
    }

    stage('DAST') {
        build 'SECURITY-DAST-Arachni'
    }
}
