final String staging_docker_host = "ssh://user123@10.10.10.131"    

node {
    stage("Build") {
        sh "docker build -t myapp:${version} ."
    }
    stage("Deploy Artifact") {
        sh "docker push -t myapp:${version}"
    }
}

stage("Staging Deploy Approval") {
    input "Deploy to Staging?"
}

node {
    stage("Deploy Stagin"){
        withEnv(["DOCKER_HOST=${staging_docker_host}"]) {
            sshagent( credentials: ['jenkins_docker']) {
                sh "docker run -d -p 80:80 myapp:${version}"
            }
        }
    }
}

stage("Production Deploy Approval") {
    input "Deploy to Prod?"
}

node {
    stage("Deploy Prod"){
        withEnv(["DOCKER_HOST=${staging_docker_host}"]) {
            sshagent( credentials: ['jenkins_docker']) {
                sh "docker -h ${prod_docker_host} run -d -p 80:80 myapp:${version}"
            }
        }
    }
}
