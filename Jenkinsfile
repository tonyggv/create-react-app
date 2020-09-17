pipeline{
    agent {
      kubernetes {
        label "sonarcloud-automation"
        idleMinutes 1440
        defaultContainer 'sonarcloud'
        namespace "cicd"
        cloud "k8scluster-infra-ops-sg"
        yaml """
          apiVersion: v1
          kind: Pod
          metadata:
            name: sonarcloud
            namespace: cicd
          spec:
            containers:
            - name: sonarcloud
              image: maven
              command:
              - cat
              tty: true
        """
      }
  }
    stages{
        stage("check version"){
             steps{
                 container("node"){
                  sh 'node --version'
                 }
            }
        }
        stage("sonarqube"){
                steps{
                    container("sonarcloud"){
                          script {
                               scannerHome = tool 'sonarqube-scanner'
                          }
                          withSonarQubeEnv('sonarcloud') { // If you have configured more than one global server connection, you can specify its name
                              sh "${scannerHome}/bin/sonar-scanner " +
                                 "-Dsonar.pullrequest.branch=${env.BRANCH_NAME} " +
                                 "-Dsonar.pullrequest.key=${env.CHANGE_ID} "  +
                                 "-Dsonar.pullrequest.provider=github "
                          }
                    }
                }
            }
    }
}