pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'maven') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'maven') {
                    sh 'mvn test'
                }
            }
        }
    }
    post {
    	echo '==========IN POST BUILD=========='
    	def changelog = gitChangelogApiBuilder()
    		.withFromCommit(ZERO_COMMIT)
   			.withToRef("refs/heads/master")
   			.withTemplatePath("changelog.mustache")
   			.toFile("CHANGELOG.md");
    }
}