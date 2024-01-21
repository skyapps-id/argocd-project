node {
    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'GITHUB_TOKEN', passwordVariable: 'GIT_TOKEN', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email ${AUTHOR_EMAIL}"
                    sh "git config user.name ${AUTHOR_NAME}"
                    sh "cat services/${SVC_NAME}/${ENV}/deployment.yaml"
                    sh "sed -i 's+image: ${IMAGE_NAME}--${ENV}.*+image: ${IMAGE_NAME}--${ENV}:${DOCKER_TAG}+g' services/${SVC_NAME}/${ENV}/deployment.yaml"
                    sh "cat services/${SVC_NAME}/${ENV}/deployment.yaml"
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job change manifest: ${env.BUILD_NUMBER}'"
                    sh('git push https://$GIT_TOKEN@github.com/$GIT_USERNAME/argocd-project.git HEAD:master')
                }
            }
        }
    }
}