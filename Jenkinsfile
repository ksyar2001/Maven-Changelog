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
    		echo "${getLastSuccessfulCommit()}"
    		
    		script {
    			def changelogtext = gitChangelog returnType: 'STRING',
    				template: '''{{#commits}}
						[{{hash}}] **{{messageTitle}}** 
						{{/commits}}'''
				currentBuild.description = changelogtext
			}
    	}
    }
}

def getLastSuccessfulCommit() {
  def lastSuccessfulBuild = currentBuild.rawBuild.getPreviousSuccessfulBuild()
  def lastSuccessfulHash = commitHashForBuild( lastSuccessfulBuild )
  return lastSuccessfulHash
}

@NonCPS
def commitHashForBuild( build ) {
  def scmAction = build?.actions.find { action -> action instanceof jenkins.scm.api.SCMRevisionAction }
  return scmAction?.revision?.hash
}

