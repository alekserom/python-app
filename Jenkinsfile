node('built-in'){
    cleanWs()
    stage("Download source code"){
       git 'https://github.com/alekserom/python-app'
    }
    stage("Deploy in kubernetes"){
        sh "helm upgrade --install python-app . --kube-insecure-skip-tls-verify --kube-apiserver=https://localhost:8443 --debug --kubeconfig=/home/alex/cert/config.json --wait"
    }
    cleanWs()
}
