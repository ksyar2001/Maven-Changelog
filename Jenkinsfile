node {

 def changelogString = gitChangelog returnType: 'STRING',
  from: [type: '', value: 'master'],
  to: [type: '', value: 'master'],
  template: """<h1> Git Changelog changelog </h1>
  {{#commits}}
	<p>
	<h3>{{{messageTitle}}}</h3>
	{{#messageBodyItems}}
 	<li> {{.}}</li> 
	{{/messageBodyItems}}
	</p>
  {{/commits}}
  """
 currentBuild.description = changelogString
}
