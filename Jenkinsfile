
node {
    
    tools {
        maven 'maven-3.6.3'
        jdk 'jdk-1.11'
    }

    stage("Git Clone"){

        git branch: 'main', url: 'https://github.com/nalajala9/spring_on_eks.git'
    }

     stage('Maven Build') {

       sh 'mvn clean package'

    }

    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t spring-boot-app .'
        sh 'docker image list'
        sh 'docker tag spring-boot-app 20152282/spring-boot-app:v1'
    }

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'docker login -u 20152282 -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'docker push spring-boot-app 20152282/spring-boot-app:v1'
    }
    
    stage("kubernetes deployment"){
        sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
    }
} 
