pipeline {
    agent any
    tools{
        maven "MAVEN3"
        jdk "OracleJDK8"
    
    }
    environment {
        SNAP_REPO = "vprofile-snapshot"
        NEXUS_USER = "admin"
        NEXUS_PASS = "sezersezer"
        NEXUS_REPO = "vporfile-release"
        CENTRAL_REPO = "vprofile-maven-central"
        NEXUSIP = "192.168.56.10"
        NEXUSPORT = "8081"
        NEXUS_GRP_REPO = "vprofile-group"
        NEXUS_LOGIN = "nexuslogin"
              
    }

    stages {
        stage("build"){
            steps{
                sh "mvn -s settings.xml -DskipTests install"
            }
        }
    }
}