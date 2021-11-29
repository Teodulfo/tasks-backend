pipeline {
    agent { label '192.168.122.20'} 
    stages {
        stage ('Build Backend'){
            steps {
                sh '/opt/tomcat/bin/catalina.sh stop'
                sh 'mvn clean package'
                sh 'cp target/tasks-backend.war /opt/tomcat/webapps/.'
                sh '/opt/tomcat/bin/catalina.sh start'
            }
        }           

    }
}