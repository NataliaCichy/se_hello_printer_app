pipeline {
    agent any
    stages {
        stage('Deps') {
            steps {
	            sh 'make deps'
        	}
        }
        stage('Test') {
          steps {
            sh 'make test_xunit || true'
            xunit thresholds: [
                skipped(failureThreshold: '0'),
                failed(failureThreshold: '1')
            ],
            tools: [
                JUnit(deleteOutputFiles: true,
                      failIfNotNew: true,
                      pattern: 'test_results.xml',
                      skipNoTestFiles: false,
                      stopProcessingIfError: true)
            ]
          }
        }
        stage('Linter') {
          steps {
            sh 'make lint'
          }
        post{
          always{
            cobertura autoUpdateHealth: false,
                      autoUpdateStability: false,
                      coderturaReportFile: 'coverage.xml',
                      conditionalCoverageTargets: '70, 0, 0',
                      failUnhealthy: false,
                      failUnstable: false,
                      lineCoverageTargets: '80, 0, 0',
                      maxNumerOfBuilds: 0,
                      methodCoverageTargets: '80, 0, 0',
                      onlyStable: false,
                      sourceEncoding: 'ASCII',
                      zoomCoverageChart: false
          }
        }
        }
    }
}
