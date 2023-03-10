pipeline {
    agent {

        docker {
            image '10.129.0.4:8123/jenkins-agent:stable'
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
                sh 'cp -r /var/lib/jenkins/workspace/build-war-file/target/hello-1.0.war /var/lib/jenkins/workspace/build-war-file'
                sh 'docker build -t $image_name /var/lib/jenkins/workspace/build-war-file'
                sh 'docker tag $image_name 10.129.0.4:8123/$image_name:$tag'
                withDockerRegistry(credentialsId: '2abc3bc9-874a-4958-8b16-a426a6f60c89', url: 'http://10.129.0.4:8123/') {
                sh 'docker push 10.129.0.4:8123/$image_name:$tag'
                }
            }
        }
        stage('Run docker on remote host') {
            steps {
                sh 'ssh-keyscan -H 10.129.0.6 >> ~/.ssh/known_hosts'
                sh '''ssh root@10.129.0.6 <<EOF
  sudo docker pull 10.129.0.4:8123/$image_name:$tag
  sudo docker run -d -t 10.129.0.4:8123/$image_name:$tag
EOF'''
                
            }
        }
    }
}