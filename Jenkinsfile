node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("layamoorthy/webpage")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * Just an example */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhubid') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
     stage('Ansible Init') {
         steps {
                script {
                
               def tfHome = tool name: 'Ansible'
                env.PATH = "${tfHome}:${env.PATH}"
                 sh 'ansible --version'
                    
            }
            }
     }
    stage('Ansible Deploy') {
             
            steps {
                 
             
               
               sh "ansible-playbook py.yml -i /home/test/ansible-nginx-demo/hosts --user jenkins --key-file ~/.ssh/id_rsa -e '/home/test/ansible-nginx-demo/files/nginx.conf.j2'"

}
}
}
