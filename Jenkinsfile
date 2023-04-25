node {
  stage ('======== Clone repository ========') {
    checkout scm
  }
  stage('======== Build image ========') {
    app = docker.build("test/nginx")
  }
  stage('======== Push image ========') {
    docker.withRegistry('https://shryu-test.kro.kr', 'Harbor') {
       app.push("${env.BUILD_NUMBER}")
       app.push("latest")
    }
  }
  stage('======== Update YAML file ========') {
    sh "git switch main"
    sh "git pull"
    sh "sed -i s%test/nginx:.*%test/nginx:${env.BUILD_NUMBER}%g nginx.yaml"
    sh "cat nginx.yaml | grep image:"
    sh "git add nginx.yaml"
    sh "git commit -m 'image tag update ${env.BUILD_NUMBER}’”
    sh "git push -u origin main"
  }
}

