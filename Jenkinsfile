pipeline {
  agent {
   label 'master'
  }
  environment {
    GOPATH = "$WORKSPACE"
    GITHUB_COMMIT="$GIT_COMMIT"
    CHANGE_ID="$CHANGE_ID"
    BRANCH_NAME="$BRANCH_NAME"
  }

  stages {
    stage("JenkinsInitHasNoDocker") {
      steps {
        withCredentials([usernamePassword( credentialsId: 'dockerhub-bloxcicd', usernameVariable: 'DOCKER_USER', passwordVariable\
: 'PASSWORD')]) {
          sh "mkdir -p ${DOCKER_CONFIG}"
          sh "echo ${PASSWORD} | docker login  -u ${DOCKER_USER} --password-stdin ${DOCKER_REGISTRY}"
        }
      }
    }
    stage("First") {
     
      steps {
        script{
        def url="$GIT_URL"
        final git_repo = url.substring(url.lastIndexOf('/') + 1, url.length())
        echo git_repo
        env.GIT_REPO =git_repo
        echo env.GIT_REPO
        echo env.GITHUB_COMMIT
        echo env.CHANGE_ID
        echo env.BRANCH_NAME
       }
       
    }
    }
    
    stage("Build CVE job"){
      steps{
    
      build job: 'run_ngp_cve_scan_image', parameters: [[$class: 'StringParameterValue', name: 'COMMIT_ID', value:env.GITHUB_COMMIT], [$class: 'StringParameterValue', name: 'GITHUB_REPO', value:env.GIT_REPO],[$class: 'StringParameterValue', name: 'CHANGE_ID', value:env.CHANGE_ID]]
      }
  }
} 
} 
