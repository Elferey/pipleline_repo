pipeline {
    agent {

        docker {
            image '10.129.0.4:8123/jenkins-agent:1'
        }
    }

    stages {
        stage ('Clone project repo') {
            steps {
                git 'https://github.com/Elferey/my_box.git'
                git 'https://github.com/Elferey/Dockerfiles.git'
            }
        }
        stage ('Build war file') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('Create and push app container') {
            steps {
                sh 'cp -r /var/lib/jenkins/workspace/pipeline1/target/hello-1.0.war /var/lib/jenkins/workspace/pipeline1'
                sh 'docker build -t $image_name /var/lib/jenkins/workspace/pipeline1'
                sh 'docker tag $image_name 10.129.0.4:8123/$image_name:$tag'
                sh 'docker push 10.129.0.4:8123/$image_name:$tag'
            }
        }
    }
}