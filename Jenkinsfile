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
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
              
    }

    stages {
        stage("build"){
            steps{
                sh "mvn -s settings.xml -DskipTests install"
            }
            post {
                success {
                    echo "now archiving"
                    archiveArtifacts artifacts: "**/*.war"
                }
            }
        }
        stage("test"){
            steps {
                sh "mvn -s settings.xml test"
            }
        }
        stage("checkstyle analysis"){
            steps{
                sh "mvn -s settings.xml checkstyle:checkstyle"
            }
        }
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
    }
}