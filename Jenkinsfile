pipeline {
    agent { label '192.168.122.20'} 
    stages {
        stage ('Build Backend'){
            steps {
                sh 'mvn clean package'
                sh 'cp target/tasks-backend.war /opt/tomcat/webapps/.'
            }
        }
        stage ('Build Frontend'){
            steps {
                    dir('frontend'){
                        git 'git@github.com:Teodulfo/tasks-frontend.git'             
                        sh 'mvn clean package'
                        sh 'cp target/tasks.war /opt/tomcat/webapps/.'
                    }
                sh '/opt/tomcat/bin/catalina.sh start &'              
            }
        }           
        stage ('Health Check'){
            steps {
                sh 'curl http://192.168.122.20:8080/tasks/ -s -f -o /dev/null'
            }
        }           

    }
}