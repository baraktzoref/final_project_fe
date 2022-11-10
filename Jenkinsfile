pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh '''
                    echo "--Running build--"
                    echo "--with npm install--"
                    DOCKER_BUILDKIT=1 docker build -f Dockerfile_ppl -t baraktzoref/final_project_fe --target bld .
                '''
            }
        }
        stage('test') {
            steps {
                sh '''
                    echo "--Running test--"
                    echo "--with npm test--"
                    DOCKER_BUILDKIT=1 docker build -f Dockerfile_ppl -t baraktzoref/final_project_fe --target tst .
                '''
            }
        }
        stage('start') {
            steps {
                sh '''
                    echo "--Running delivery--"
                    echo "--with npm start--"
                    DOCKER_BUILDKIT=1 docker build -f Dockerfile_ppl -t baraktzoref/final_project_fe --target dlv .
                '''
            }
        }
        stage('run') {
            steps {
                sh '''
                    echo "--Activate docker container--"
                    docker run -d baraktzoref/final_project_fe
                '''
            }
        }
        stage('push-image-to-repo') {
            steps {
                sh (script:"set +x && docker login -u ${username} -p ${pass} && set -x")
                sh '''
                    echo "--Push docker image--"
                    docker tag baraktzoref/final_project_fe baraktzoref/final_project_fe:jenkins-${BUILD_NUMBER}
                    
                    docker push baraktzoref/final_project_fe --all-tags
                    
                '''
            }
        }
        stage('cleanup') {
            steps {
                sh '''
                    echo "-----Running docker prune-----"
                    docker system prune -f
                '''
            }
        }
    }
}
