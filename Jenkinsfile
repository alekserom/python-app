node('built-in'){
    cleanWs()
    stage("Download source code"){
       git 'https://github.com/alekserom/python-app'
    }
    stage("check SAST"){
        withCredentials([string(credentialsId: 'sonar-token', variable: 'token')]) {
            sh "sonar-scanner \
                -Dsonar.projectKey=my-python-app \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=${token}"
        }
    }
    stage("Compilation app"){
        sh"""pyinstaller --add-data "templates/profile.html:." --add-data "templates/vulnerable.html:." --add-data "templates:templates/"  sql-injection.py"""
    }
    stage("Build image"){
        sh "docker build -t python-app ."
    }
    stage("Tag image"){
        sh "docker tag python-app registry.local:9001/docker-local/python-app:01.000.00"
    }
    stage("Registry login"){
        withCredentials([usernamePassword(credentialsId: 'nexus-cred', passwordVariable: 'pass', usernameVariable: 'user')]) {
            sh "docker login -u ${user} -p ${pass} registry.local:9001"
        }
    }
    stage("Push image in registry"){
        sh "docker push registry.local:9001/docker-local/python-app:01.000.00"
        sh "docker system prune -a"
    }
    cleanWs()
}
