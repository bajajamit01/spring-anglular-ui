
podTemplate(containers: [containerTemplate(image: 'docker:17.12.0-ce-dind', name: 'docker', privileged: true, ttyEnabled: true)]){
	podTemplate(containers: [containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-kubectl:v1.19.11', command: 'cat', ttyEnabled: true)]){
     node (POD_LABEL) {
        stage('Get a Spint-boot-UI project') {
            git 'https://github.com/bajajamit09/spring-anglular-ui.git'
            container('docker') {

                stage('Build Image') {
                   sh "ls -l"
                   sh "docker build -t k8workshopregistry.azurecr.io/spring-ui ."
                   sh "docker images"
                }
          }
               container('docker') {

                stage('Build Docker Image') {
                sh "docker login -u k8workshopregistry k8workshopregistry.azurecr.io -p RnQA8Y+AMxdNBT3jbNLINocGdCMGVd5R"
                sh "docker push k8workshopregistry.azurecr.io/spring-ui"
//	      dockerImage = docker.build("hello-world-java")
		    }
	}
             container('helm'){
              stage('Deploy image'){
              sh "kubectl apply -f ./spring-boot-ui.yaml"
              sh "kubectl get pods"
              }
	    }

            }
        }
    }
}
}
