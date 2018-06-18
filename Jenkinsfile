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
				currentBuild.description = getlogText();
				
				def existing_releasenote = fileExists 'release-notes.md'
				if (existing_releasenote) {
					sh "mv release-notes.md release-notes-previous.md"
					writeFile encoding: 'utf-8', file: 'release-notes.md', text: trimmedtext
					sh "cat release-notes-previous.md >> release-notes.md"
					pushreleaseNotes();
				} else {
					writeFile encoding: 'utf-8', file: 'release-notes.md', text: trimmedtext
					pushreleaseNotes();
				}
			}
    	}
    }
}

def getlogText() {
	def changelogtext = gitChangelog returnType: 'STRING',
    				from: [type: "COMMIT", value: env.GIT_PREVIOUS_SUCCESSFUL_COMMIT],
    				template: '''{{#commits}}
	    					{{^merge}}
								[{{hash}}] **{{messageTitle}}**
							{{/merge}}
						{{/commits}}'''
	return trimlogText(changelogtext)
}

def trimlogText(String log_text) {
	def lines = log_text.split('\n')
	def trimmed_lines = []
	trimmed_lines.add("##Release Notes for build: ${env.BUILD.NUMBER}")
	lines.each {
	    line -> trimmed_lines.add(line.trim());
	}
	def trimmedtext = trimmed_lines.join("\n")
	return trimmedtext
}

def pushreleaseNotes() {
	sh "git add release-notes.md"
    sh "git commit -m 'adding a new release-notes'"
    sh "git push origin HEAD:master"
}
