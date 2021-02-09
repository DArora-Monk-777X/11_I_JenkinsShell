pipeline {
    agent any

    stages {
        stage('Upload') {
            steps {
                echo 'Building..'
                sh 'docker-compose --version'
                echo 'Uploading ...'
                sh '''
                    rm -rf workspace
                    mkdir workspace
                    cd workspace
                    ie-app-publisher-linux ws init
                    cd ..
                    cp -RT app ./workspace
                    cd workspace
                    ie-app-publisher-linux de c -u http://localhost:2375
                    export IE_SKIP_CERTIFICATE=true
                    ie-app-publisher-linux em li -u "$IEM_URL" -e $USER_NAME -p $PSWD
                    ie-app-publisher-linux em app cuv -a $APP_ID -v 0.0.$BUILD_NUMBER -y ./docker-compose.prod.yml -n '{"hello-edge":[{"name":"hello-edge","protocol":"HTTP","port":"80","headers":"","rewriteTarget":"/"}]}' -s 'hello-edge' -t 'FromBoxReverseProxy' -u "hello-edge" -r "/"
                    ie-app-publisher-linux em app uuv -a $APP_ID -v 0.0.$BUILD_NUMBER
                '''

            }
        }
      
    }
}

