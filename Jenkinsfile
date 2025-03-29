node {
    def app
    stage('Clone repository') {
        checkout scm
    }

    // check if the current branch is the 'dev' branch
    if (env.BRANCH_NAME == 'dev') {
        stage('Build image') {
            app = docker.build("milakirovski/kiii-jenkins")
        }

        stage('Push image') {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                app.push("${env.BRANCH_NAME}-latest")
                // signal the orchestrator that there is a new version
            }
        }
    } else {
        echo "Skipping Docker build and push because this is not the 'dev' branch (current: ${env.BRANCH_NAME})"
    }
}
