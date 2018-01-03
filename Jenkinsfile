node {

    def git_creds = 'gitlab-reg-credentials'
    
    def app
    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "registry.gitlab.com/amjidi/kubernetes-pipeline/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build" 
    
    sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
      
    /*app = docker.build(${imageName}, "-f applications/hello-kenzan/Dockerfile applications/hello-kenzan")
    */
    
    
  stage('Push') { 
        
    /*    docker.withRegistry('https://registry.gitlab.com/amjidi/kubernetes-pipeline', 'gitlab-reg-credentials')  
        app.push()
    */
    
   withCredentials([
      [$class: 'UsernamePasswordMultiBinding', credentialsId: git_creds, usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS'],
  ]){
       
    sh "docker login registry.gitlab.com -u ${GIT_USER} -p ${GIT_PASS}"
    
    sh "docker push ${imageName}"
   
   }
   }

    
  /*  
    stage "Deploy" {

        sh "sed 's#registry.gitlab.com/amjidi/kubernetes-pipeline/hello-kenzan:latest#'$BUILDIMG'#' applications/hello-kenzan/k8s/deployment.yaml | kubectl apply -f -"
        sh "kubectl rollout status deployment/hello-kenzan"

    }
    */
}
