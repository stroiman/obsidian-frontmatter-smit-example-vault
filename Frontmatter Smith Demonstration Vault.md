This vault is meant to show how the [[Frontmatter Smith]] plugin works. [Official URL](https://github.com/stroiman/obsidian-frontmatter-smith)

Remember to enable plugins when opening this vault, see [[Plugins enabled in this vault]]

## Usage

### Run the plugin command.

Open a page, e.g. a daily note, and run the "Forge frontmatter" command

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

