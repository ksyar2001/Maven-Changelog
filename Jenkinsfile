node {

 def changelogString = gitChangelog returnType: 'STRING',
  from: [type: 'REF', value: 'master'],
  to: [type: 'REF', value: 'master'],
  template: """<h1> Git Changelog changelog </h1>"""
 currentBuild.description = changelogString
}
