node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALSS', url: 'https://github.com/manikarnam/playjenkins.git',branch: 'master'
    }
  stage('Build Docker Image'){
	    sh "sudo docker build -t maniengg/webappmss:${BUILD_ID} ."
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "sudo docker login -u maniengg -p ${DOKCER_HUB_PASSWORD}"
        }
        sh "sudo docker push maniengg/webappmss:${BUILD_ID}"
     }
  stage("Deploy To Kuberates Cluster"){
        withCredentials([file(credentialsId: 'demo-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
         sh "gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
         sh "gcloud config set project mssdevops-284216"
         sh "gcloud config set compute/zone us-central1-c"
         sh "gcloud config set compute/region us-central1"
         sh "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project mssdevops-284216"
         sh "sed -i -e 's,image_to_be_deployed,' maniengg/webappmss:${BUILD_ID}',g' myweb.yaml"
         sh "kubectl apply -f myweb.yaml"
        }
      }
}
