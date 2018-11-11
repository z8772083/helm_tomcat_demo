node{
   
    stage('Git Clone From Github'){
        git branch: 'master', url: 'https://github.com/z8772083/javahometech.git'
    }
   
    stage('Maven Build'){
        def MvnHome = tool name: 'maven', type: 'maven'
        def MvnCmd = "${MvnHome}bin/mvn"
        sh "${MvnCmd} -Dskiptest clean package"
    }

    stage('Build and Push image'){
        sh """
        docker build -t ${repo}/dev/${app}:${BUILD_NUMBER} .
        docker login ${repo} -u admin -p ${Docker_hub}
        docker push ${repo}/dev/${app}:${BUILD_NUMBER}
        """
    }
   
    stage('Deploy'){
    sh """
    helm --host ${helm_host} upgrade --install --wait  --set image.repository=${repo}/dev/${app},image.tag=${BUILD_NUMBER} hello hello
    """
    }
           
}
