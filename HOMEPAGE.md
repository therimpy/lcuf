---
banner: "![[Elysee Map.png]]"
banner_y: 0.445
---
## <center>The Lavitra Chronicles:</center>
## <center><font size=35>**UNCHARTED FRONTIERS**</font></center>
```dataviewjs
// Set path and class
const vault = this.app.vault.adapter.getResourcePath("").split("?")[0];
dv.container.className += ' hideSort cards cards-cover cards-1-1';

// Display PC card table
dv.table(["cover", "name", "details"],
  dv.pages(`"For Players/Party"`)
  .sort(page => page.file.name, "asc")
    .map(p => [
      `![](${vault}/${p.cover})`, // For image link use: [![](${vault}/${p.cover})](<${p.file.name}#${p.file.name}>)
      p.name,
      obsidian.Platform.isMobile ? `:LiCircleUserRound: ${p.race}<br>:LiSwords: ${p.class}<br>:LiEye: ${p.subclass}` : `:LiCircleUserRound: ${p.race} / :LiSwords: ${p.class} / :RiEyeCloseLine: ${p.subclass}`
    ])
);
```

> [!columns|2]
> > [!noted|blank]
> > # Session Notes
> > ```dataview
> > TABLE WITHOUT ID
> > 	sessionnum AS ":LiBookmark:",
> > 	link(file.link, name) AS Title,
> > 	game_date AS ":LiCalendarFold:",
> > 	summary AS ":CoNoteEdit:"
> > FROM "For Players/Sessions"
> > SORT sessionNum DESC
> > LIMIT 6
> > ```
>
> > [!noted|blank]
> > # Ongoing Quests
> > ```dataviewjs
> > let result = await dv.query(`LIST FROM "Compendium/Quest Log" AND #quest/ongoing`);
> > let values = result.value.values;
> > let embeds = values.map(p => "[" + dv.fileLink(p.path, true) + "](<" + p.path + ">)");
> > dv.el("p", embeds, { cls: "dataview-cards-deck" });
> > ```

# Activity
```dataview  
TABLE WITHOUT ID  
link(file.path, file.folder + " / " + file.name) AS "Page",  
file.mtime AS "Last Modified"  
FROM "Compendium" OR "For Players"  
WHERE file.mtime >= date(today) - dur(30 days)  
AND file.name != this.file.name  
AND !contains(file.path, "z_Assets")  
AND !contains(file.path, "Inline Scripts")  
AND !contains(file.path, "z_Templates")  
AND !contains(file.path, "daily notes")  
AND !contains(file.path, "BRAT")  
SORT file.mtime DESC  
LIMIT 10  
```