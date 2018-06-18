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
    	success {
    		echo "${env.GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
    		echo "${env.BUILD_NUMBER}"
    		
    		script {
    			def changelogtext = gitChangelog returnType: 'STRING',
    				from: [type: "COMMIT", value: env.GIT_PREVIOUS_SUCCESSFUL_COMMIT],
    				template: '''{{#commits}}
						[{{hash}}] **{{messageTitle}}** 
						{{/commits}}'''
				currentBuild.description = changelogtext
				def exist = fileExists 'release-notes.md'
				if (exist) {
					echo 'APPENDING FILE'
				} else {
					writeFile encoding: 'utf-8', file: 'relase-notes.md', text: changelogtext
					sshagent (credentials: ['admin']) {
                    sh "git commit -a -m 'Adding a release-note'"
                    sh "git push"
                    }	  
				}
			}
    	}
    }
}
