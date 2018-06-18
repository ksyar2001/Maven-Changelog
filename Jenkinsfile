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
				def lines = changelogtext.split('\n')
				def trimmed_lines = []
				lines.each {
				    line -> trimmed_lines.add(line.trim());
				}
				def trimmedtext = trimmed_lines.join("\n")
				currentBuild.description = trimmedtext
				
				def exist = fileExists 'release-notes.md'
				if (exist) {
					sh "mv release-notes.md release-notes-previous.md"
					writeFile encoding: 'utf-8', file: 'release-notes.md', text: trimmedtext
					sh "cat release-notes-previous.md >> release-notes.md"
					sh "git add release-notes.md"
					sh "git commit -m 'appending to the release-notes'"
					sh "git push origin HEAD:master"
				} else {
					writeFile encoding: 'utf-8', file: 'release-notes.md', text: trimmedtext
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
