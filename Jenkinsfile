node {
    def app
    
    // ========== REPLACE THESE VALUES ========== //
    def gitUsername   = "your-github-username"     // 👈 ACTUAL GITHUB USERNAME
    def gitEmail      = "your-email@example.com"   // 👈 ACTUAL EMAIL
    def dockerhubUser = "your-dockerhub-id"        // 👈 ACTUAL DOCKERHUB ID
    // ========================================= //
    
    stage('Clone repository') {
        checkout scm
    }
    
    stage('Update GIT') {
        script {
            try {
                withCredentials([usernamePassword(
                    credentialsId: 'github', 
                    passwordVariable: 'GIT_PASSWORD', 
                    usernameVariable: 'GIT_USERNAME'
                )]) {
                    // Configure Git
                    sh "git config --global user.email '${gitEmail}'"
                    sh "git config --global user.name '${gitUsername}'"
                    
                    // Update image tag in deployment.yaml
                    sh "sed -i 's|YOUR-DOCKERHUB-USERNAME/1tierapp.*|${dockerhubUser}/1tierapp:${env.BUILD_NUMBER}|g' deployment.yaml"
                    
                    // Commit and push changes
                    sh "git add ."
                    sh "git commit -m 'Jenkins auto-update: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                }
            } catch (err) {
                error "Git update failed: ${err.message}"
            }
        }
    }
}
