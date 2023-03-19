node {
    stage ('Git Checkout'){
        git 'https://github.com/formycore/mrdevopscicd.git'
    }
    stage ('Docker Build'){
        sh "docker image build -t $JOB_NAME:v1.$BUILD_ID ."
        // what if the we change the code and run this code again we need to change the 
        // build id these are the jenkins variable 
        sh "docker image tag $JOB_NAME:v1.$BUILD_ID formycore/$JOB_NAME:v1.$BUILD_ID"
        // changes we done needs to be done on the latest one
        sh "docker image tag $JOB_NAME:v1.$BUILD_ID formycore/$JOB_NAME:latest"
    }
    stage ('Docker push'){
        // with pipeline syntax use with credentials bind
        withCredentials([string(credentialsId: 'dockerpass', variable: 'dockerpassword')]) {
        sh "docker login -u formycore -p ${dockerpassword}"
        // we are pushing with version and latest as both are same only
        sh "docker image push formycore/$JOB_NAME:v1.$BUILD_ID"
        sh "docker image push formycore/$JOB_NAME:latest"
        // after pushing to the docker why we need the images locally 
        // we are removing the docker images with versions,latest,tagged one also
        sh "docker rmi  $JOB_NAME:v1.$BUILD_ID formycore/$JOB_NAME:v1.$BUILD_ID formycore/$JOB_NAME:latest"
        }
    }
    stage('Docker Deployment'){
        def docker_run = 'docker run -itd --name webserver -p 9000:80 formycore/mrdevops'
        def docker_rm = 'docker rm -f webserver'
        def docker_rmi = 'docker rmi -f formycore/mrdevops'
        sshagent(['webserver']) {
            sh "ssh -o StrictHostKeyChecking=no maanya@10.128.0.11 ${docker_rm}"
            sh "ssh -o StrictHostKeyChecking=no maanya@10.128.0.11 ${docker_rmi}"
            sh "ssh -o StrictHostKeyChecking=no maanya@10.128.0.11 ${docker_run}"
            
            

    }
    }
}