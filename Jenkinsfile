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
        ansiblePlaybook credentialsId: 'e9744cfc-988e-47ed-bfa5-bdf8ed2b0fe1', installation: 'Anisble', playbook: '$WORKSPACE/createInstance.yaml'
      }
   }
   stage('DeployArtifact') {
      steps {
        ansiblePlaybook become: true, credentialsId: 'e9744cfc-988e-47ed-bfa5-bdf8ed2b0fe1', installation: 'Anisble', inventory: '/tmp/hosts_eureka', extras: '-e WORKSPACE=$WORKSPACE', playbook: '$WORKSPACE/deployArtifact.yaml'
      }
   }
    stage('BuildDownstream') {
      steps {
        build job: 'OMS_CUSTOMER', parameters: [string(name: 'EUREKA_IPADDRESS', value: '$EUREKA_IPADDRESS')]
      }
    }
  }
}
