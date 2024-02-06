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
				script {
					ChecksDetails details = new ChecksDetailsBuilder()
						.withName("Jenkins CI")
						.withStatus(ChecksStatus.COMPLETED)
						.withConclusion(ChecksConclusion.SUCCESS)
						.withDetailsURL(DisplayURLProvider.get().getRunURL(run))
						.withCompletedAt(LocalDateTime.now(ZoneOffset.UTC))
						.build();
					ChecksPublisher publisher = ChecksPublisherFactory.fromRun(run);
					publisher.publish(details);
				}
			}

		}
	}
}
