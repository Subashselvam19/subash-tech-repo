pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh '''
                    if sudo docker container inspect nginx >/dev/null 2>&1; then
                        sudo docker stop nginx || true
                        sudo docker rm nginx || true
                    else
                        echo "Container nginx does not exist."
                    fi

                    if sudo docker image inspect nginx:latest >/dev/null 2>&1; then
                        sudo docker rmi nginx:latest || true
                    else
                        echo "Image nginx:latest does not exist."
                    fi

                    sudo docker pull nginx:latest
                '''
            }
        }

        stage('deploy') {
            steps {
                sh '''
                    sudo docker run -d \
                        -p 80:80 \
                        -v "$(pwd)/index.html:/usr/share/nginx/html/index.html:ro" \
                        --name nginx \
                        nginx:latest
                '''
            }
        }
    }
}
