pipeline {
  agent any
  tools { 
        maven 'Maven'
  }
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
    }
    stage('CreateInstance') {
      steps {
        ansiblePlaybook credentialsId: 'd28aea44-7963-408d-99b0-cafacde1fd4c', installation: 'Anisble', playbook: '$WORKSPACE/createInstance.yaml'
      }
   }
   stage('DeployArtifact') {
      steps {
        ansiblePlaybook become: true, credentialsId: 'd28aea44-7963-408d-99b0-cafacde1fd4c', installation: 'Anisble', inventory: '/tmp/hosts', playbook: '$WORKSPACE/deployArtifact.yaml'
      }
   }

  }
}
