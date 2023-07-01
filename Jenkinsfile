def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger'
]
pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment {
        SNAP_REPO = "vprofile-snapshot"
        NEXUS_USER = "admin"
        NEXUS_PASS = "sezersezer"
        NEXUS_REPO = "vprofile-release"
        CENTRAL_REPO = "vprofile-maven-central"
        NEXUSIP = "192.168.56.10"
        NEXUSPORT = "8081"
        NEXUS_GRP_REPO = "vpro-maven-group"
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
        REGISTRY_CREDENTIAL = 'docker-hub'
        AWS_CREDENTIAL = 'ecs'
        AWS_REGION = 'eu-west-1'
        ECS_CLUSTER = 'vprofiletest'
        ECS_SERVICE = 'vprofiletestservice'
        DOCKER_IMAGE = 'sezrsezr/vprofile:latest'
    }

    stages {
        
        stage('deploy container to testenv') {
            steps {
                script {
                    withAWS(credentials: 'ecs', region: 'eu-west-1') {
                        sh "aws ecs update-service --cluster ${ECS_CLUSTER} --service ${ECS_SERVICE} --force-new-deployment"
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'slack notifications'
            slackSend channel: '#cicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build: ${env.BUILD_NUMBER} \n more info at: ${env.BUILD_URL}"
        }
    }
}
