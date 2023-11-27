pipeline{
agent any
  options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
}

triggers {
  pollSCM '* * * * *'
}

tools{
    maven "maven1"
}
stages{
    stage("Initialization") {
            steps {
                // use name of the patchset as the build name
                buildName "${GERRIT_CHANGE_SUBJECT}"
                buildDescription "Executed @ ${NODE_NAME}"
            }
        }
    stage("checkout"){
        steps{
            git 'https://github.com/rkumar701/maven-web-application.git'
        }
    }
    stage("build"){
        steps{
            sh 'mvn clean package'
        }
    }
    stage("test"){
        steps{
            sh 'mvn sonar:sonar'
        }
    }
    stage("deploy"){
        steps{
            sshagent(['tomcat']) {
            sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@54.144.244.5:/opt/apache-tomcat-9.0.83/webapps'
        }
        }
    }
    
}//stages end
}//pipeline end
