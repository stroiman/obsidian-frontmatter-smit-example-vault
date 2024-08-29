This vault is meant to show how the [[Frontmatter Smith]] plugin works. [Official URL](https://github.com/stroiman/obsidian-frontmatter-smith)

Remember to enable plugins when opening this vault, see [[Plugins enabled in this vault]]

This page shows two use cases
- Basic User Case - the plugin is triggered by a user action
- Templater integration - the plugin is triggered automatically from a templater template.

## Basic Use Case

This is the standard use case. You have a note open for which you want to add some standard frontmatter properties. A core use case for this plugin is to add metadata to the daily notes.

### Run the plugin command.

Open a note, e.g. the daily note, and run the "Forge frontmatter" command

![[Run the command.png]]

### Selecting a forge.

A forge is a set of rules for creating frontmatter. You can configure multiple forges for different types of configuration.

You will be prompted which forge to use:


![[Selecting a forge.png]]

> [!note]
> If there is only one forge configured in the configuration; this prompt is not shown, and the single forge is used.

> [!note] No sleep patterns
> Only examples with medicine is added in this vault. The extra sleep pattern was just added as an example of how multiple forges work.

### Selecting data

Here, the "Medicine usage" forge was selected. This queries for three pieces of information. The type of medicine, the dose, and the time of consumption.

![[Entering type.png]]

![[Entering dose.png]]

![[Entering time.png]]

### The data is added

Complex data is not shown in a pretty way in Obsidian, but all the data is there

![[Frontmatter example.png]]

Opening the note in "Source Mode" will show that the data is structured as valid YAML data:

```
---
medicin-log:
  - type: "[[Methylphenidate]]"
    dose: 10mg
    time: 08:00
  - type: "[[Lisdexamfetamine]]"
    dose: 20mg
    time: 08:00
---
```

> [!info] Frontmatter links are automatically updated
> When you rename a note, links in the frontmatter is automatically updated (if you have enabled the setting). Even in this case with complex frontmatter, containins lists of objects.

> [!caution] Renaming does not update plugin configuration
> While Obsidian does handle updating links in the frontmatter, Obsidian has no knowledge of the structure of the plugin configuration. If you rename a note used in the Frontmatter smith configuration; you will have to update the configuration manually.

## Viewing the data

The following dataview query shows that while Obsidian may not display the complex data structures well in the frontmatter editor, all information is fully supported and available in e.g. dataview queries. 

This query was written in javascript to nicely present the recorded information. You could easily add more information to the table, e.g. sleep to see how it correlates with your sleep pattern; or a note of behavioural patterns.

```dataviewjs
const days = {
'0': 'Sunday',
'1': 'Monday',
'2': 'Tuesday',
'3': 'Wednesday',
'4': 'Thursday',
'5': 'Friday',
'6': 'Sunday'
}
const pages = dv.pages('"Journal"').where(x => x['medicin-log']).sort(x => x.file.name, "desc")
const mapped = pages.map(p => {
const medicin = p['medicin-log']
const meds = medicin.map(medicin => {
const { type, dose, time } = medicin
return `${type} (${dose}${time ? ` @ ${time}` : ''})`

})
const day = new Date(p.file.name).getDay()
return[
p.file.link, days[day], meds
]})
dv.table(["File", "Day", "Medicin"], mapped)
```

## Simple API, e.g. for Templater integration

You can programatically trigger this plugin. One use case for this is to run it automatically from a [Templater](https://silentvoid13.github.io/Templater/) template

The folder "Templater Integration" is setup with an automatic template that runs a Frontmatter Smith forge.

Try following this link to a non, existing file; and the smith will be run by the template. Both the template and the forge generates frontmatter, and the two are merged together.

[[Templater integration/New link in the vault]]