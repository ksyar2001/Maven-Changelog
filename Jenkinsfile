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
				changelogtext.each {
				    line -> echo '${line}'
				}

				currentBuild.description = changelogtext
				def exist = fileExists 'release-notes.md'
				if (exist) {
					echo 'APPENDING FILE'
				} else {
					writeFile encoding: 'utf-8', file: 'release-notes.md', text: changelogtext
					sshagent (credentials: ['admin']) {
                    sh "git add release-notes.md"
                    sh "git commit -m 'adding a new release-notes'"
                    sh "git push origin HEAD:master"
                    }	  
				}
			}
    	}
    }
}
