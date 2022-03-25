pipeline {
    agent any
    tools { 
        maven 'Maven' 
      
    }
stages { 
     
    stage('Build') {
       steps {
        sh 'mvn -Dmaven.test.failure.ignore=true install'
              }

   }
 
  stage('Unit Test Results') {
      steps {
      junit '**/target/surefire-reports/TEST-*.xml'
      
      }
         
     stage('Artifact upload') {
      steps {
     nexusPublisher nexusInstanceId: '1234', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'gameoflife-web/target/gameoflife.war']], mavenCoordinate: [artifactId: 'gameoflife', groupId: 'com.wakaleo.gameoflife', packaging: 'war', version: '$BUILD_NUMBER']]]
      }
  }
    stage('Deploy War') {
      steps {
          //deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://52.14.128.173:8080/')], contextPath: null, war: '**/*.war'
          //sh label: '', script: 'ansible-playbook deploy-withinfra.yml'
          sh label: '', script: 'ansible-playbook deploy.yml'
      }
  }

post {
        success {
            archiveArtifacts 'gameoflife-web/target/*.war'
        }
        failure {
            mail to:"dkotesh@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
        }
    }
           
    }
}
