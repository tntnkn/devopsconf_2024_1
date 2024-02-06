pipeline {
	agent any

	stages {
		stage("Checkout") {
		steps {
		dir("${BUILD_TAG}") {
			checkout scmGit(
				branches: [[name: 'master']],
				userRemoteConfigs: [[url: 'https://github.com/tntnkn/devopsconf_2024_1.git']])
		}}}
		stage("Analyze") {
			steps {
				echo "TEST TEST!"
			}
		}
	}
}
