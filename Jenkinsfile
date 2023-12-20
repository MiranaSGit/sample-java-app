pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: maven
            image: maven:alpine
            command:
            - cat
            tty: true
        '''
    }
  }
    stages {
        stage('BUILD'){
            steps {
             container('maven') {
                sh 'mvn clean install -DskipTests'
             }    
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    	stage('UNIT TEST'){
            steps {
             container('maven') {
                sh 'mvn test'
             }
            }
        }

	stage('INTEGRATION TEST'){
            steps {
             container('maven') {
                sh 'mvn verify -DskipUnitTests'
             }
            }
        }
		
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
             container('maven') {
                sh 'mvn checkstyle:checkstyle'
             }
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
    }
}
