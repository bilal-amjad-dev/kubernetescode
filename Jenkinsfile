node {
    def app
    
    // ========== ONLY CHANGE REQUIRED ========== //
    def dockerImage = "your-dockerhub-username/1tierapp"  // 👈 Replace with your DockerHub username
    // ========================================= //

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build(dockerImage) 
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
        echo "triggering updatemanifestjob"
        build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}

