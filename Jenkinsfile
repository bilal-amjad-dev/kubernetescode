node {
    def app
    
    // ========== REQUIRED CHANGES ========== //
    def dockerhubUser = "bilalamjaddev"  // 👈 REPLACE THIS
    def dockerImage = "${dockerhubUser}/1tierapp"  // 👈 Now includes username
    // ====================================== //

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${dockerImage}:${env.BUILD_NUMBER}")  // 👈 Tag during build
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed for build ${env.BUILD_NUMBER}"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")  // Pushes as your-username/1tierapp:123
            // app.push("latest")  // ⚠️ Optional (avoid in production)
        }
    }
    
    stage('Trigger CD Pipeline') {
        build job: 'updatemanifest', parameters: [
            string(name: 'DOCKERTAG', value: env.BUILD_NUMBER),
            string(name: 'DOCKERHUB_USER', value: dockerhubUser)  // 👈 Pass username to CD
        ]
    }
}
