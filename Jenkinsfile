node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        sh '''
        $ curl -H "Content-Type: application/json" --data '{"build": true}' -X POST https://registry.hub.docker.com/u/kirtikumar1982/hellotestone/trigger/339dc38b-7184-4db5-ab7a-d4c820d68dfa/
        sh '''
        
        app = docker.build("kirtikumar1982/hellotestone")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         *  */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'kirtikumar1982', 'Secure+800') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
