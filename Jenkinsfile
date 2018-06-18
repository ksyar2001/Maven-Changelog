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
    	always {
    		echo 'IN POST SCRIPT'
    		gitChangelog from: [type: 'COMMIT', value: 'fa97be'],
			returnType: 'STRING', template: '''{{#commits}}
				**{{messageTitle}}** 
				{{/commits}}''',
			to: [type: 'COMMIT', value: 'fa265a8']
    	}
    }
}

