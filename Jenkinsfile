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
    		echo "${currentBuild.description}"
    		script{
    			currentBuild.description = gitChangelog from: [type: 'COMMIT', value: 'fa97be'],
				returnType: 'CONTEXT', template: '''{{#commits}}
					**{{messageTitle}}** 
					{{/commits}}''',
				to: [type: 'COMMIT', value: 'fa265a8']   
    		}
    	}
    }
}

