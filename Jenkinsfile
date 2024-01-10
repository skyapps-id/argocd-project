node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'GITHUB_TOKEN', passwordVariable: 'GIT_TOKEN', usernameVariable: 'GIT_USERNAME')]) {
                    //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                    sh "git config user.email aji.indrajaya@efishery.com"
                    sh "git config user.name 'Aji Indra Jaya'"
                    sh "cd services/${SVC_NAME}"
                    //sh "git switch ${BRANCH}"
                    sh "cat deployment.yaml"
                    sh "sed -i 's+echo-ci-cd.*+echo-ci-cd:${DOCKERTAG}+g' deployment.yaml"
                    sh "cat deployment.yaml"
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_TOKEN}@github.com/${GIT_USERNAME}/argocd-project.git HEAD:master"
                }
            }
        }
    }
}