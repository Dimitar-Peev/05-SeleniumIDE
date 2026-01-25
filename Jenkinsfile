pipeline {
    agent any

    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }
        }

        stage('Restore the project') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build the project') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Run tests') {
            steps {
                bat 'dotnet test --no-build --configuration Release --logger "trx;LogFileName=test_results.trx" --results-directory TestResults'
            }
        }
		
		stage('Publish Artifacts') {
            steps {
                bat 'dotnet publish SeleniumIDE/SeleniumIde.csproj -c Release -o publish'
                archiveArtifacts artifacts: 'publish/**', fingerprint: true
            }
        }
    }
	
    post {
        always {
            echo 'Pipeline completed'
			
            // Archive the raw .trx test result for inspection
            archiveArtifacts artifacts: 'TestResults/*.trx', fingerprint: true

            // Optional: If you install and use trx2junit, convert to XML and use:
            // junit 'TestResults/*.xml'

            cleanWs()
        }
		
        success {
            echo 'Build succeeded'
        }
		
        failure {
            echo 'Build failed'
        }
    }
}