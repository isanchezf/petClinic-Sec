pipeline {

    agent any

    environment {
        skipDeploy = setSkipDeploy()
        BRANCH = "${env.gitlabSourceBranch}"
    }

    stages {

        stage('Environment'){
            steps{
	            echo "SkipDeploy: ${skipDeploy}"
	            echo "BRANCH: ${env.gitlabSourceBranch}"
                echo "Hello World Ivan"
	        }
        }
        
        stage('Build') {
            steps {
                echo 'Build'
                sh "mvn --batch-mode package" 
            }
        }

        stage('Archive Unit Tests Results') {
            steps {
                echo 'Archive Unit Test Results'
               step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/TEST-*.xml'])
            }
        }
        
        stage('Publish Unit Test results report') {
            steps {
                echo 'Report'
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: false, reportDir: 'target/site/jacoco/', reportFiles: 'index.html', reportName: 'jacaco report', reportTitles: ''])

             }
        }
        
//         stage('SonarQube Analysis') {
//             tools {
//                jdk 'JDK 11.0.1'
//             }
//             steps {
//                 withSonarQubeEnv('SonarQube Steps Community') {
//                   script {
//                     sh "mvn clean package sonar:sonar \
//   -Dsonar.projectKey=GSCORAM-SPETCLINIC \
//   -Dsonar.projectName=sPetClinic \
//   -Dsonar.host.url=https://umane.emeal.nttdata.com/sonarqubece \
//   -Dsonar.login=sqp_84dfd1b1a0e19103bf77222c55c23b98d9105c00 \
//   -Dsonar.token=sqp_84dfd1b1a0e19103bf77222c55c23b98d9105c00 \
//   -Dsonar.branch.name=${BRANCH} \
//   -Dsonar.sources=src/main \
//   -Dsonar.java.binaries=target/classes" 
//                     }
//                 }
//             }
//         }
        stage('Deploy'){
            when { expression { !skipDeploy } }
            steps{
                echo 'Realizo despliegue exitosamente'
            }
        }
    }
}

def setSkipDeploy() {
	if ("${env.gitlabSourceBranch}".contains('hotfix') || 
        "${env.gitlabSourceBranch}".contains('develop') || 
        "${env.gitlabSourceBranch}".contains('release') ||
        "${env.gitlabSourceBranch}".contains('master')) {
		return false
	}
    return true
}