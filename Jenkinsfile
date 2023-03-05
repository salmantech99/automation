pipeline {
    agent any

    environment {
        SFCC_CLIENT_ID = 'your_sfcc_client_id'
        SFCC_CLIENT_SECRET = 'your_sfcc_client_secret'
        SFCC_ORGANIZATION_ID = 'your_sfcc_organization_id'
        SFCC_USER_NAME = 'your_sfcc_username'
        SFCC_PASSWORD = 'your_sfcc_password'
        SFCC_CODE_VERSION = 'your_sfcc_code_version'
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh 'sfcc-ci auth:login --client-id=$SFCC_CLIENT_ID --client-secret=$SFCC_CLIENT_SECRET --instance-url=https://$SFCC_ORGANIZATION_ID.demandware.net --user-name=$SFCC_USER_NAME --password=$SFCC_PASSWORD'
                sh "sfcc-ci code:deploy --code-version=$SFCC_CODE_VERSION --source-path=dist --instance-url=https://$SFCC_ORGANIZATION_ID.demandware.net"
            }
        }
    }

    post {
        always {
            junit 'test-results/**/*.xml'
        }
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        branchDiscovery {
            criteria {
                expression('feature/.*')
            }
        }
    }
}
