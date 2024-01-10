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
                    //sh "git switch ${BRANCH}"
                    sh "cat services/${SVC_NAME}/deployment.yaml"
                    sh "sed -i 's+echo-ci-cd.*+echo-ci-cd:${DOCKERTAG}+g' services/${SVC_NAME}/deployment.yaml"
                    sh "cat services/${SVC_NAME}/deployment.yaml/deployment.yaml"
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_TOKEN}@github.com/${GIT_USERNAME}/argocd-project.git HEAD:master"
                }
            }
        }
    }
}