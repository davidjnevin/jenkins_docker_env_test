podTemplate(containers: [
    containerTemplate(
        name: 'jnlp',
        image: 'jenkins/inbound-agent:latest'
        )
  ]) {

    node(POD_LABEL) {
      stages {
        stage("verify tooling") {
          steps {
            sh '''
              docker version
              docker info
              docker compose version
              curl --version
              jq --version
            '''
          }
        }
        stage('Prune Docker data') {
          steps {
            sh 'docker system prune -a --volumes -f'
          }
        }
        stage('Start container') {
          steps {
            sh 'docker compose up -d --no-color --wait'
            sh 'docker compose ps'
          }
        }
        stage('Run tests against the container') {
          steps {
            sh 'curl http://localhost:3000/param?query=demo | jq'
          }
        }
    }
}

