pipeline{
    agent {
      kubernetes {
        label "sonar-scanner"
        idleMinutes 1440
        defaultContainer 'sonar-scanner'
        namespace "cicd"
        cloud "k8scluster-infra-ops-sg"
        yaml """
          apiVersion: v1
          kind: Pod
          metadata:
            name: sonar-scanner
            namespace: cicd
          spec:
            containers:
            - name: sonar-scanner
              image: sonarsource/sonar-scanner-cli
              command:
              - cat
              tty: true
        """
      }
  }
    stages{
        stage("Scan Code"){
                steps{
                    container("sonar-scanner"){
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