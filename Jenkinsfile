node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("har12345/example-app")
    }

    stage('Test'){
            app.inside{
                      sh 'npm test'
                     }
     } 

    stage('Push image') {
        /* Finally, we'll push the image into Docker Hub */

        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
           
            /* We'll push the image with two tags:
             * First, the branch name and the latest tag
             * Second, the branch name and the incremental build number
             * Pushing multiple tags is cheap, as all the layers are reused. */
             
            app.push("${env.BRANCH_NAME}-latest")
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
        }
    }
}
