node ('agent1') {
    def app
    stages {
        stage('Cloning Git') {
            checkout scm
        }
        stage('Build-and-Tag') {
            app = docker.build("tarasovkir/sudoku")
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
    }
}
