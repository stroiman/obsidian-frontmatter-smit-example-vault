---
date: <% tp.date.now() %>
---
# <% tp.file.title %>

<%*
const file = tp.config.target_file
const folder = file.parent.name;
const api = app.plugins.plugins['frontmatter-smith'].api
const forge = api.findForgeByName('Templater integration')
if (forge) {
  // runOnFile returns a promise, but don't `await` it here. Then templater
  // will not complete until _after_ we modified the file, and a race condition 
  // will cause the one's changes overwritten by the other.
  forge.runOnFile(file)
}
-%>
