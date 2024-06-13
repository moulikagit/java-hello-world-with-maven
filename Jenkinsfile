pipeline {
    agent any  
    environment {
        JAVA_HOME = tool 'java-21'
    }

    tools {
        maven 'maven'
        jdk 'java-21'
    }

    stages {
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github access', url: 'https://github.com/moulikagit/java-hello-world-with-maven.git']]])
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcatServer', usernameVariable: 'tomcat', passwordVariable: 's3cret')]) {
                    sh "mvn tomcat7:deploy -Dtomcat.url=http://3.85.95.203:8080/manager/text -Dtomcat.username=$TOMCAT_USER -Dtomcat.password=$TOMCAT_PASS -DskipTests=true"
                }
            }
        }
    }
}
