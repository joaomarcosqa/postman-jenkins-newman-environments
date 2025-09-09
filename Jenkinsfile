pipeline {
  agent any

  tools { nodejs 'NodeJS_LTS' }

  options {
    timestamps()
    ansiColor('xterm')
  }

  environment {
    COLLECTION = 'tests/collection.postman_collection.json'
    REPORT_DIR = 'results'
  }

  stages {
    stage('Instalar Newman + htmlextra') {
      steps {
        sh '''
          npm --version
          node --version
          npm install -g newman newman-reporter-htmlextra
          newman -v
        '''
      }
    }

    stage('Executar Matrix por ambiente') {
      matrix {
        axes {
          axis {
            name 'ENV_NAME'
            values 'dev', 'stg' // adicione 'prd' quando quiser
          }
        }
        stages {
          stage('Rodar Collection') {
            steps {
              sh '''
                set -e
                ENV_FILE="tests/env.${ENV_NAME}.postman_environment.json"
                test -f "${COLLECTION}" || { echo "Collection não encontrada: ${COLLECTION}"; exit 2; }
                test -f "${ENV_FILE}"   || { echo "Ambiente não encontrado: ${ENV_FILE}"; exit 3; }

                mkdir -p ${REPORT_DIR}/${ENV_NAME}
                newman run "${COLLECTION}" \
                  -e "${ENV_FILE}" \
                  --reporters cli,junit,htmlextra \
                  --reporter-junit-export="${REPORT_DIR}/${ENV_NAME}/report.xml" \
                  --reporter-htmlextra-export="${REPORT_DIR}/${ENV_NAME}/report.html"
              '''
            }
          }
        }
        post {
          always {
            junit "results/${ENV_NAME}/report.xml"
            archiveArtifacts artifacts: "results/${ENV_NAME}/*", onlyIfSuccessful: false
          }
        }
      }
    }
  }
}

