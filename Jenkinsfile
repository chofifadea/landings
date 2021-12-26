pipeline {    
  agent { label 'master' }    
  options {    
    buildDiscarder(logRotator(numToKeepStr: '5'))    
  }        
  stages {
    

    stage('Build Go') {	
      steps {	
	      script {
              if (env.BRANCH_NAME == 'development'){
                  sh 'docker build -t gcr.io/wlb-dev/taufik-test:0.1.$BUILD_NUMBER-dev .'
              } else if (env.BRANCH_NAME == 'staging'){
                  sh 'docker build -t gcr.io/wlb-dev/taufik-test:0.1.$BUILD_NUMBER-staging .'
              } else if (env.BRANCH_NAME == 'prod'){
                  sh 'docker build -t gcr.io/wlb-dev/taufik-test:0.1.$BUILD_NUMBER-prod .'
              }
              else {
                  sh 'echo no branches'
              }
          }		        	
      }	
    }	
    stage('Publish to GCR') {	
	    steps {
	      script {
              if (env.BRANCH_NAME == 'development'){
                  sh 'docker push gcr.io/wlb-dev/taufik-test:0.1.$BUILD_NUMBER-dev'
              } else if (env.BRANCH_NAME == 'staging'){
                  sh 'docker push gcr.io/wlb-dev/taufik-test:0.1.$BUILD_NUMBER-staging'
              }
              else if (env.BRANCH_NAME == 'prod'){
                  sh 'docker push gcr.io/wlb-dev/taufik-test:0.1.$BUILD_NUMBER-prod'
              }
          }        
      }          
    }
   }
     post {
            success {
                mattermostSend channel: '#dev-ops',
                color: 'good',
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            }    

            failure {
                mattermostSend channel: '#dev-ops',
                color: 'danger',
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            }
        }
  }	
