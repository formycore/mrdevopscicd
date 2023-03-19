node {
    stage ('Git Checkout'){
        git 'https://github.com/formycore/mrdevopscicd.git'
    }
    stage ('Docker Build'){
        sh "docker image build -t $JOB_NAME:v1.$BUILD_ID ."
    }
}