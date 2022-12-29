pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
      stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run -it -v "$PWD:/pwd" ghcr.io/trufflesecurity/trufflehog:latest github --repo https://github.com/trufflesecurity/test_keys --debug --json https://github.com/msystec/jenkins-webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
        }
      }
    }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
   }
  }
 }
}

