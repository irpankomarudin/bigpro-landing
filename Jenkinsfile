env.DOCKER_REGISTRY = 'irpank'
env.DOCKER_IMAGE_NAME = 'landingpage-app'
pipeline {
    agent any
    stages {
        stage('docker build') {
            steps {
                sh "docker build --build-arg APP_NAME=$DOCKER_IMAGE_NAME -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER ."
                }
           }
        stage('Docker push') {
            steps {                
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
                }
           }
        stage('tagging') {
            steps {
                sh('sed -i "s/tag/$BUILD_NUMBER/g" kube-landing.yml')
                }
           }
        stage('locate namespace') {
            steps {
                sh('sed -i "s/default/staging/g" kube-landing.yml')
                }
           }
        stage('add domain') {
            steps {
                sh('sed -i "s/landing.komarudins.online/landing-st.komarudins.online/g" kube-landing.yml')
                }
           }
        stage('deploy') {
            steps {
                sh('kubectl apply -f kube-landing.yml')
                }
           }
        stage('remove image docker') {
            steps {
                sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER"
                }
           }
         stage('show ingress') {
            steps {
                sh('kubectl get ingress -n=staging')
                }
           }        
      }
}
