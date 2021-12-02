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
            }
        }           
        stage ('Health Check'){
            steps {
                sh 'psql postgresql://tribunais:senha@192.168.122.123:5432/tasks -c "SELECT version();"'
                sh 'ps aux | grep -v grep | grep -iv catalina && /opt/tomcat/bin/catalina.sh start' 
                sleep 10 
                sh 'curl http://localhost:8080/tasks/ -s -f -o /dev/null'
            }
        }           
         stage ('Deploy to homologacao'){
            steps {
                sh 'ssh root@192.168.122.123 "ps aux | grep -v grep | grep -ic catalina && /opt/tomcat/bin/catalina.sh stop"' 
                sh 'scp /home/teo/workspace/ProjetoPipeLine/frontend/target/tasks.war  root@192.168.122.123:/opt/tomcat/webapps/.'
                sh 'scp /home/teo/workspace/ProjetoPipeLine/target/tasks-backend.war  root@192.168.122.123:/opt/tomcat/webapps/.'
                sh 'ssh root@192.168.122.123 "/opt/tomcat/bin/catalina.sh start"' 
                sleep 10 
                sh 'curl http://192.168.122.123:8080/tasks/ -s -f -o /dev/null'
            }
        }           

    }
}