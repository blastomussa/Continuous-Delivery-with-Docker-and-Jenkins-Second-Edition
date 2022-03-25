podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: centos
        image: centos
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
''') {
  node(POD_LABEL) {
    stage('Rolling Upgrade') {
      git 'https://github.com/blastomussa/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
      container('centos') {
        stage('Init Kubectl') {
          sh '''
          curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
          chmod +x ./kubectl
          mv kubectl /usr/local/bin/kubectl
          '''
        }
        stage('Apply Changes') {
          sh '''
          cd Chapter08/sample1
          kubectl apply -f calculator.yaml -n staging
          kubectl apply -f hazelcast.yaml -n staging
          '''
        }  
      }
    }
  }
}
