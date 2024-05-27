# Notes
This is a template for a character sheet.
It includes the ability to calculate the modifier for a skill based on the ability score, proficiency, and expertise.

## Plugins
This template uses the following plugins:
- `Dataview` - This plugin allows for javascript to be run in the markdown file. You need to enable javascript and allow inline scripts in the settings. Please be careful with this, as it can be a security risk to run foreign scripts in your vault.
- `MetaEdit` - This plugin allows for the editing of frontmatter data in the markdown file.

## Themes
This template uses the following themes:
- `ITS Theme` - This theme provides the styling for the template.

```js
const meta = this.app.plugins.plugins["metaedit"].api; 
const fm = dv.current().file.frontmatter
function stat(num){
	return Math.floor((num-10)/2)
}
function calcSkillMod(stat, isProficient, isExpert, proficiency){
	let modifier = stat; 
		if (isProficient) { 
			modifier += isExpert ? (proficiency * 2) : proficiency; 
		} 
	return modifier;
}

async function setValue(skill, value){
	await meta.update(skill, value, dv.current().file.path)
}

const str = stat(fm.strength)
const dex = stat(fm.dexterity)
const con = stat(fm.constitution)
const int = stat(fm.intelligence)
const wis = stat(fm.wisdom)
const cha = stat(fm.cha)
const prof = fm.proficiency

const isAcrobaticsProficient = fm.acrobaticsProficiency
const isAcrobaticsExpert = fm.acrobaticsExpertise
const acrobaticsMod = calcSkillMod(dex, isAcrobaticsProficient, isAcrobaticsExpert, prof)
await setValue("acrobatics", acrobaticsMod)
```

```markdown
| Skill      | Value | Proficency? | Expertise? |
| ---------- | ----- | ----------- | ---------- |
| Acrobatics | `$= dv.current().file.frontmatter.acrobatics`      |  `$= dv.current().file.frontmatter.acrobaticsProficiency`             |      `$=  dv.current().file.frontmatter.acrobaticsExpertise`      |
```
