pipeline {
    agent {

        docker {
            image '10.129.0.4:8123/jenkins-agent2:latest2'
            args '--privileged -v /var/run/docker.sock:/var/run/docker.sock -u root'
        }
    }

    stages {
        stage ('Clone project repo') {
            steps {
                git 'https://github.com/Elferey/my_box.git'
            }
        }
        stage ('Build war file') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('Create and push app container') {
            steps {
                sh 'ls -la /var/run/'
                sh 'cp -r /var/lib/jenkins/workspace/build-war-file/target/hello-1.0.war /var/lib/jenkins/workspace/build-war-file'
                sh 'docker build -t $image_name /var/lib/jenkins/workspace/build-war-file'
                sh 'docker tag $image_name 10.129.0.4:8123/$image_name:$tag'
                sh 'docker push 10.129.0.4:8123/$image_name:$tag'
            }
        }
    }
}