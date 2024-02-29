node {
  stage('SonarQube analysis') {
    def scannerHome = tool name: 'sonartest', type: 'hudson.plugins.sonar.SonarRunnerInstallation';
    withSonarQubeEnv('SonarQube') { 
      sh "${scannerHome}/bin/sonartest"
    }
  }
}