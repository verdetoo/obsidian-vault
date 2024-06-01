---
banner:
---
# Table of contents
```table-of-contents
title: 
style: nestedList
minLevel: 2
maxLevel: 2
includeLinks: true
debugInConsole: false
```

# Zachyoung.dev templater snippets

## Create file if it doesn’t exist

This script will create a file if the file doesn’t already exist.

```
<%*
const fileName = "This is the name of a file";
const existing = tp.file.find_tfile(fileName);
if (!existing) {
  await tp.file.create_new("Contents", fileName);
}
_%>
```

Here’s how you can create an internal link in a template that will create a new file only if it doesn’t already exist.

```
<%*
const fileName = "This is the name of a file";
const existing = tp.file.find_tfile(fileName);
let createdFileDisplay;
if (existing) {
  createdFileDisplay = existing.basename;
} else {
  createdFileDisplay = (await tp.file.create_new("Contents", fileName)).basename;
}
_%>

// Somewhere later in the file
[[<% createdFileDisplay %>]]
```

Here’s a script that will create an internal link in a template that will create a new file using a template if it doesn’t already exist.

```
<%*
const fileName = "This is the name of a file";
const existing = tp.file.find_tfile(fileName);
let createdFileDisplay;
if (existing) {
  createdFileDisplay = existing.basename;
} else {
  createdFileDisplay = (await tp.file.create_new(tp.file.find_tfile("template-name"), fileName)).basename;
}
_%>

// Somewhere later in the file
[[<% createdFileDisplay %>]]
```

## Reapply template instead of append

Place this script at the top of your template to clear out the file first before applying the rest of your template.

This is useful if you want to “reapply” a template to a file rather than append to it.

```
<%*
await app.vault.modify(tp.file.find_tfile(tp.file.path(true)), "");
_%>
// Rest of template below
```

## Generating links

The simplest way to generate a link to another file is to use the standard wikilink syntax. This is often sufficient, and what I typically use in my example templates.

```
<%*
// Somehow get the name of the file you want to link to (suggester, prompt, computed from a method, etc.)
const link = "People/Jake";
-%>
[[<% link %>]]
```

However, if you have the same filename in multiple folders, this won’t always work as you expect. If you often have multiple files with the same name in different folders, you should use the `app.fileManager.generateMarkdownLink()` Obsidian API. This API will take into account your settings under “Files & Links.”

```
<%*
const file = tp.file.find_tfile("People/Jake");
const link = app.fileManager.generateMarkdownLink(file, tp.file.folder(true));
-%>
<% link %>
```

You can also use this API to generate links to headings/blocks, or for setting display text for the link.

```
<%*
const file = tp.file.find_tfile("People/Jake");
const link = app.fileManager.generateMarkdownLink(
  file, // file
  tp.file.folder(true), // current folder
  "#Summary", // heading
  "Jake's Summary", // display text
);
-%>
<% link %>
```

## Create link to previously open file

`app.workspace.getLastOpenFiles()` provides a list of filenames of the previously open files. We can use this to generate a link to the previously open file. You can use this in combination with [folder templates](https://silentvoid13.github.io/Templater/settings.html#folder-templates) to automatically generate links to the file that created the link when creating files from clicking on links to non-existing notes.

```
<%*
const parentFile = tp.file.find_tfile(app.workspace.getLastOpenFiles()[0]);
const parentLink = app.fileManager.generateMarkdownLink(parentFile, tp.file.folder(true));
-%>
---
parent: "<% parentLink %>"
---
```

## Create links to all files created today

This script will create a list of all files created today, based on a field in your notes frontmatter. This could be modified to use `cday`, but I’ve found that to not be accurate, especially when syncing files between devices, so I prefer to store the created/modified dates of notes in frontmatter of the note.

You can swap out the `createdOn` in `fileCache?.frontmatter?.createdOn` with whatever field you want put in your frontmatter to track the created date. You can also change `YYYY-MM-DD` to match the date format you put for the value of your created date field.

```
<%*
const today = tp.date.now("YYYY-MM-DD");
// Filter out files that shouldn't be included
const filteredFiles = app.vault.getMarkdownFiles().filter(file => {
  const fileCache = app.metadataCache.getFileCache(file);
  // If the file has a createdOn field and it's value is today, include it
  return fileCache?.frontmatter?.createdOn === today;
});
// Convert list of files into list of links
const links = filteredFiles.map(file => `[[${file.basename}]]`).join("\n");
tR += links;
_%>
```

## Create link to last active file

When creating a new note, if you want to be able to link to the note you were on before you created the note, you can use this. `app.workspace.lastActiveFile.basename` should represent the file you were viewing at the time of executing the template. Unsure of how consistently this will work.

```
up:: [[<% app.workspace.lastActiveFile.basename %>]]
```

## Opening files in new tabs

This script will open multiple tabs of notes. You can do repeat these two lines of code as much as you’d like.

```
<%*
const note1 = tp.file.find_tfile("filename1");
app.workspace.getLeaf(true).openFile(note1);

const note2 = tp.file.find_tfile("filename2");
app.workspace.getLeaf(true).openFile(note2);
-%>
```

This script will open multiple tabs and create notes with templates applied to them. For the last tab we don’t need to explicitly open the tab.

```
<%*
const template1 = tp.file.find_tfile("template-1");
await tp.file.create_new(template1, "filename1", true);
const note1 = tp.file.find_tfile("filename1");
app.workspace.getLeaf(true).openFile(note1);

const template2 = tp.file.find_tfile("template-2");
await tp.file.create_new(template2, "filename2", true);
const note2 = tp.file.find_tfile("filename2");
app.workspace.getLeaf(true).openFile(note2);

const template3 = tp.file.find_tfile("template-3");
await tp.file.create_new(template3, "filename3", true);
-%>
```

## Daily notes block that only shows on certain day of week

The following daily notes template has two blocks. The first block will only show on daily notes that land on a Monday. The second block will show on every daily note.

```
<%* if (tp.date.now("dddd", 0, tp.file.title, "YYYY-MM-DD") === "Monday") { %>
Block that only shows on Mondays
<%* } %>
Block that shows every day
```

## Create links to all weekly notes in current month

This script will create a list of links to all weekly notes that fall within the current month, using the current note as a reference. Assumes that the monthly note format of `YYYY-MM` and a weekly note format of `YYYY-[W]ww`.

```
<%*
// Get year and month from note name, assumes format of "YYYY-MM"
const [year, month] = tp.file.title.split("-");

// Momentjs works with months starting at 0 index (January is month 0, February is month 1, etc)
// Subtract one to convert to starting at 0 index
const monthZeroIndex = month - 1;

// Get number of days in month
const monthDate = moment([year, monthZeroIndex]);
const daysInMonth = monthDate.daysInMonth();

// For each day in month
const weeks = [];
for (let dayOfMonth = 1; dayOfMonth <= daysInMonth; ++dayOfMonth) {
  // Get week for day of month
  const week = moment([year, monthZeroIndex, dayOfMonth]).week();

  // If week is a new week, add to list of weeks for month
  if (!weeks.some(x => x === week)) {
    weeks.push(week);
  }
}

// Format weeks to be links, formatted at "[[YYYY-[W]WW]]"
const weeksFormatted = weeks.map(week => (
  `[[${year}-W${String(week).padStart(2, "0")}]]`
)).join("\n");
-%>

<% weeksFormatted %>
```

## Create links to all daily notes in week

This script will create a list of links to all daily notes that fall within the current week, using the current note as a reference. Assumes that the weekly note format of `YYYY-[W]ww` and a daily note format of `YYYY-MM-DD`.

```
[[<% tp.date.weekday("YYYY-MM-DD", 0, tp.file.title, "YYYY-[W]ww") %>]]
[[<% tp.date.weekday("YYYY-MM-DD", 1, tp.file.title, "YYYY-[W]ww") %>]]
[[<% tp.date.weekday("YYYY-MM-DD", 2, tp.file.title, "YYYY-[W]ww") %>]]
[[<% tp.date.weekday("YYYY-MM-DD", 3, tp.file.title, "YYYY-[W]ww") %>]]
[[<% tp.date.weekday("YYYY-MM-DD", 4, tp.file.title, "YYYY-[W]ww") %>]]
[[<% tp.date.weekday("YYYY-MM-DD", 5, tp.file.title, "YYYY-[W]ww") %>]]
[[<% tp.date.weekday("YYYY-MM-DD", 6, tp.file.title, "YYYY-[W]ww") %>]]
```

## Create link to random note in folder

We can use `Math.random()` to get a random number to get a link to a random note in a folder.

```
<%*
const folder = "Quotes";
const quotes = app.vault.getMarkdownFiles().filter(x => x.path.startsWith(folder));
const randomQuote= quotes[Math.floor(Math.random() * quotes.length)].basename;
-%>

[[<% randomQuote %>]]
```

## Include random line from another file

We can use `tp.file.include()` to get the contents of a file, then split it into lines and use `Math.random()` to get a random line from that file.

```
<%*
// Get file content
const file = tp.file.find_tfile("Tables/Background");
const content = await tp.file.include(file);
// Split file content into lines, ignoring empty lines
const lines = content.split('\n').filter(line => line.trim() !== '');
// Grab a random line
const line = lines[Math.floor(Math.random() * lines.length)];
-%>
<% line %>
```

## Get current week of month

Gets the current week of the month. Assumes that weeks start on a Sunday. If you want weeks to start on a Monday instead, use `isoWeek` instead of `week`.

```
<%-*
const momentObj = moment(tp.file.title);
const firstDayOfMonth = momentObj.clone().startOf("month");
const firstDayOfWeek = firstDayOfMonth.clone().startOf("week");
const offset = firstDayOfMonth.diff(firstDayOfWeek, "days");
const weekOfMonth = Math.ceil((momentObj.date() + offset) / 7);
-%>
<% weekOfMonth %>
```

## Infinite prompt until no value

Sometimes you want to prompt for multiples of the same thing, but you’re not sure how many you’ll need to prompt for. You can use a `while` loop to prompt multiple times and stop prompting once you don’t provide a value.

This example prompts for a task over and over until you don’t provide a value.

```
<%*
let isAddingTasks = true;
while (isAddingTasks) {
  const task = await tp.system.prompt("Enter a task");
  if (task) {
-%>
- [ ] <% task %>
<%*
  } else {
    isAddingTasks = false;
  }
}
-%>
```

## Suggester with “on the fly” value

There’s not built in support for using `tp.system.suggester()` and typing out an option that doesn’t exist in the list of items. However, we can support this by checking to see if you hit the `Escape` key, and if so, prompt for a custom option.

```
<%*
const items = ["item 1", "item 2"];
let selectedItem = await tp.system.suggester(item => item, items);
// No selected option, user hit `Escape` key
if (!selectedItem) {
  selectedItem = await tp.system.prompt("Type custom option");
}
-%>
<% selectedItem %>
```

We could also add a custom option to the list of items, and if it’s selected, prompt for a custom option.

```
<%*
const customOption = "--CREATE CUSTOM OPTION--"
const items = ["item 1", "item 2", customOption];
let selectedItem = await tp.system.suggester(item => item, items);
if (selectedItem === customOption) {
  selectedItem = await tp.system.prompt("Type custom option");
}
-%>
<% selectedItem %>
```

## Suggester for files with tag

This script will show a modal with a searchable list of files with a specific tag.

```
<%*
const tag = "#example-tag";

const filteredFiles = app.vault.getMarkdownFiles().filter(file => {
  const tags = tp.obsidian.getAllTags(app.metadataCache.getFileCache(file));
  return tags.includes(tag);
});
const selectedFile = (await tp.system.suggester((file) => file.basename, filteredFiles)).basename;
-%>

[[<% selectedFile %>]]
```

## Suggester for tags

This script will show a modal with a searchable list of tags you can select from.

```
<% tp.system.suggester(item => item, Object.keys(app.metadataCache.getTags())) %>
```

This variation will remove the `#` symbol from the tags, for use in YAML (since the `#` symbol denotes a comment and will not work in YAML).

```
<% tp.system.suggester(item => item, Object.keys(app.metadataCache.getTags()).map(x => x.replace("#", ""))) %>
```

## Get most recently modified file with specific tag

This script will give you a link to the most recently modified file in your vault with a specific tag.

```
<%*
// Set tag you want to get latest file for here
const tag = "#example-tag";

const latestTFileWithTag = app.vault.getMarkdownFiles().reduce((currLatestTFileWithTag, file) => {
  // Get all tags for file we're currently checking
  const tags = tp.obsidian.getAllTags(app.metadataCache.getFileCache(file));

  // If file has tag and if that file was modified more recently than the currently found most recently modified file, then set most recently modified file to file
  if (tags.includes(tag) && (!currLatestTFileWithTag || currLatestTFileWithTag.stat.mtime < file.stat.mtime)) {
    currLatestTFileWithTag = file;
  }
  return currLatestTFileWithTag;
}, null);

// Get basename of TFile to be used in link
const latestFileWithTag = latestTFileWithTag.basename;
-%>
[[<% latestFileWithTag %>]]
```

If you want to include nested tags (for example, have `#example-tag` also search files that have the tag `#example-tag/nested`), then you can switch out this condition

```
tags.includes(tag)
```

with this condition.

```
tags.some(t => t.startsWith(tag))
```

## Get most recently created file in a folder

This script will give you a link to the most recently created file in a given folder.

```
<%*
// Set folder you want to get latest file for here
const folder = "30 Terms";
// Get all files in that folder, including nested folders
const filesInFolder = app.vault.getMarkdownFiles().filter(file => file.path.startsWith(folder));
// Sort files by ctime
filesInFolder.sort((a, b) => a.stat.ctime < b.stat.ctime ? 1 : -1);
// Get basename of latest TFile to be used in link
const latestFileName = filesInFolder[0].basename;
-%>
[[<% latestFileName %>]]
```

I prefer using frontmatter to set the created/modified dates in files, since `ctime` and `mtime` rely on the OS to set those dates and the OS sometimes doesn’t set it or resets it when syncing files across devices. Here’s an example where we use a `createdAt` property in frontmatter instead.

```
<%*
// Set folder you want to get latest file for here
const folder = "30 Terms";
// Set property key to use instead of ctime
const createdAtKey = "createdAt";
const latestFileInFolder = app.vault.getMarkdownFiles().reduce((acc, file) => {
  // Skip files not in folder
  if (!file.path.startsWith(folder)) {
    return acc;
  }
  // Get time file was created from frontmatter
  const createdAt = app.metadataCache.getFileCache(file)?.frontmatter?.[createdAtKey];
  // If file has created at frontmatter and if that file was created more recently than the currently found most recently created file, then set most recently created file to file
  if (createdAt && (!acc || new Date(createdAt).getTime() > new Date(acc.createdAt).getTime())) {
    acc = { file, createdAt };
  }
  return acc;
}, null)?.file;
// Get basename of latest TFile to be used in link
const latestFileName = latestFileInFolder.basename;
-%>
[[<% latestFileName %>]]
```

## Suggester for files in a specific folder

We use `app.vault.getMarkdownFiles()` to get all the markdown files in the vault, then `.filter()` them down to only files in a specific folder. Then use `tp.system.suggester` using those markdown files.

```
<%*
const folder = "Folder Name";
const items = app.vault.getMarkdownFiles().filter(x => x.path.startsWith(folder));
const selectedItem = (await tp.system.suggester((item) => item.basename, items)).basename;
-%>

[[<% selectedItem %>]]
```

## Suggester for folders

We can use `app.vault.getAllLoadedFiles()` to get files and folders in the vault (also known as `TAbstractFile`), then filter down to just folders by checking if the `TAbstractFile` is an instance of a `TFolder`.

```
<%*
// Get all folders
const items = app.vault.getAllLoadedFiles().filter(x => x instanceof tp.obsidian.TFolder);
// Prompt user to select folder
const selectedItem = (await tp.system.suggester((item) => item.path, items)).path;
// Move current file to be in selected folder
if (selectedItem) {
  await tp.file.move(`${selectedItem}/${tp.file.title}`);
}
-%>
```

## Suggester for subfolders in a specific folder

We use `app.vault.getAbstractFileByPath()` to get a `TAbstractFile` folder object, then show a prompt to the user for subfolders in that folder. If there are no subfolders or if no subfolder is selected, then prompt for a new subfolder to be created. Then move the current file to the subfolder.

```
<%*
// Get details about folder (change "Work/Meetings" to desired folder)
const meetingsFolder = "Work/Meetings";
const meetingsTFolder = app.vault.getAbstractFileByPath(meetingsFolder);
const companies = meetingsTFolder.children.filter(subfolder => subfolder instanceof tp.obsidian.TFolder);
let selectedCompany;

// Prompt user to select company if there are any companies
if (companies.length > 0) {
  selectedCompany = (await tp.system.suggester((company) => company.name, companies))?.name;
}

// If no company selected or no companies to select from, prompt for new company
if (!selectedCompany) {
  selectedCompany = await tp.system.prompt("New company");
}

// Move file to company folder, creating the company folder if needed
await tp.file.move(`${meetingsFolder}/${selectedCompany}/${tp.file.title}`);
-%>
```

## Suggester for files with aliases

We use `app.vault.getMarkdownFiles()` to get all the markdown files in the vault. For each file, get the aliases (if there are any) and set them as the display property on the file. Then use `tp.system.suggester` using those markdown files, showing the display text, which includes the aliases. This will also let you search by alias.

```
<%*
const files = app.vault.getMarkdownFiles().reduce((acc, file) => {
  const aliases = app.metadataCache.getFileCache(file)?.frontmatter?.aliases;
  if (Array.isArray(aliases)) {
    aliases.forEach(alias => {
      const linkText = app.fileManager.generateMarkdownLink(file, tp.file.folder(true), null, alias);
      acc.push({
        display: `${alias}\n${file.path}`,
        linkText,
      });
    });
  } else if (aliases && typeof aliases === "string") {
    const linkText = app.fileManager.generateMarkdownLink(file, tp.file.folder(true), null, aliases);
    acc.push({
      display: `${aliases}\n${file.path}`,
      linkText,
    });
  }
  const linkText = app.fileManager.generateMarkdownLink(file, tp.file.folder(true), null, file.basename);
  acc.push({
    display: `${file.basename}\n${file.path}`,
    linkText,
  });
  return acc;
}, []);
const selectedItem = (await tp.system.suggester(item => item.display, files)).linkText
-%>

<% selectedItem %>
```

## Suggester for files that match frontmatter metadata

We can use `app.metadataCache.getFileCache()` to get the file cache, which includes the frontmatter, for a given markdown file. Then check to see if it matches the key/value we’re searching for.

```
<%*
// Setup key and value to search for
const key = "author";
const value = "Billy Joel"
// Loop through all markdown files in vault
const filteredFiles = app.vault.getMarkdownFiles().filter(file => {
  // Get value in frontmatter for specified key above
  const foundValue = app.metadataCache.getFileCache(file)?.frontmatter?.[key];
  // If file doesn't have matching frontmatter key or value is empty, skip
  if (!foundValue) {
    return false;
  }
  // If frontmatter is array of values, check if any of the values match
  if (Array.isArray(foundValue)) {
    return foundValue.includes(value);
  }
  // If frontmatter is not an array, check if value matches exactly
  return foundValue === value;
});
// Prompt user to select file
const selectedFile = (await tp.system.suggester((file) => file.basename, filteredFiles)).basename;
-%>
[[<% selectedFile %>]]
```

## Suggester for files that are in folder that also match frontmatter metadata

We can combine the techniques learned from creating a suggester for [files in a specific folder](https://zachyoung.dev/posts/templater-snippets#suggester-for-files-in-a-specific-folder) and creating a suggester for [files that match frontmatter metadata](https://zachyoung.dev/posts/templater-snippets#suggester-for-files-that-match-frontmatter-metadata).

```
<%*
// Setup folder and key/value pair to search for
const folder = "40 Sources"
const key = "author";
const value = "Billy Joel"
// Loop through all markdown files in vault
const filteredFiles = app.vault.getMarkdownFiles().filter(file => {
  // Check if file is in folder, if not, then we can skip frontmatter check
  const matchesFolder = file.path.startsWith(folder);
  if (!matchesFolder) {
    return false;
  }
  // Get value in frontmatter for specified key above
  const foundValue = app.metadataCache.getFileCache(file)?.frontmatter?.[key];
  // If file doesn't have matching frontmatter key or value is empty, skip
  if (!foundValue) {
    return false;
  }
  // If frontmatter is array of values, check if any of the values match
  if (Array.isArray(foundValue)) {
    return foundValue.includes(value);
  }
  // If frontmatter is not an array, check if value matches exactly
  return foundValue === value;
});
// Prompt user to select file
const selectedFile = (await tp.system.suggester((file) => file.basename, filteredFiles)).basename;
-%>
[[<% selectedFile %>]]
```

If you’re using Dataview inline metadata, you’ll have to use Dataview’s APIs to query your metadata instead of Obsidian’s built in APIs.

```
<%*
// Use DataviewAPI
const dv = DataviewAPI;
// Setup folder and key/value pair to search for
const folder = '"40 Sources"';
const key = "Author";
const value = "Billy Joel"
// Loop through all markdown files in vault using dv
const filteredFiles = dv.pages(folder).where(p => {
  const foundValue = p[key];
  // If file doesn't have matching frontmatter key or value is empty, skip
  if (!foundValue) {
    return false;
  }
  // If frontmatter is array of values, check if any of the values match
  if (Array.isArray(foundValue)) {
    return foundValue.includes(value);
  }
  // If frontmatter is not an array, check if value matches exactly
  return foundValue === value;
}).file.name;
// Prompt user to select file
const selectedFile = (await tp.system.suggester((file) => file, filteredFiles));
-%>
[[<% selectedFile %>]]
```

## Suggester for files that are in folder that also have specific outlink

An example of an outlink would be if I had a note called “Note 1” with the following contents

```
I am linking to [[Note 2]]
```

then “Note 1” would have an outlink to “Note 2”.

We can check the `links` property on the result of `app.metadataCache.getFileCache()` to find all outlinks for a given file, and filter down files based on that list.

```
<%*
// Setup folder and outlink to search for
const folder = "People";
const outlink = "Jake";
// Loop through all markdown files in vault
const filteredFiles = app.vault.getMarkdownFiles().filter(file => {
  // Check if file is in folder, if not, then we can skip outlink check
  const matchesFolder = file.path.startsWith(folder);
  if (!matchesFolder) {
    return false;
  }
  // If no outlinks, skip outlink check
  const links = app.metadataCache.getFileCache(file)?.links;
  if (!links || links.length === 0) {
    return false;
  }
  // Check if any outlinks match
  return links.some(({ link }) => link.endsWith(outlink));
});
// Prompt user to select file
const selectedFile = await tp.system.suggester((file) => file.basename, filteredFiles);
const link = app.fileManager.generateMarkdownLink(selectedFile, tp.file.folder(true), null, selectedFile.basename);
-%>
<% link %>
```

## Using tp.file.include in a user script

In order to use `tp.file.include` in a user script, you must return the result of `tp.file.include` at the end of the function. The `return` keyword is important.

```
function test(tp) {
  return tp.file.include("[[test-template]]");
}

module.exports = test;
```

```
<% tp.user.test(tp) %>
```

## Daily note links to yesterday and tomorrow

We can pass in a format, offset, reference, and reference format to [tp.date.now](https://silentvoid13.github.io/Templater/internal-functions/internal-modules/date-module.html#tpdatenowformat-string--yyyy-mm-dd-offset-numberstring-reference-string-reference_format-string) to create links to yesterday and tomorrow relative to the active daily note, instead of being relative to the actual current date.

```
[[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>|yesterday]]

[[<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>|tomorrow]]
```

Here’s a more advanced example that uses [moment](https://momentjs.com/) directly and reuses the moment object (using `.clone()`) and format.

```
<%*
// Define the moment once
const now = moment(tp.file.title, 'Do dddd');
// Define the format once
const linkFormat = '[01 Daily Notes]/YYYY/MM MMMM/Do dddd|dddd, Do';
const previousDayLink = now.clone().subtract(1, 'd').format(linkFormat);
const nextDayLink = now.clone().add(1, 'd').format(linkFormat);
-%>
[[<% previousDayLink %>]] | [[<% nextDayLink %>]]
```

Though this does work, I personally prefer dynamic links using dataview, which I have documented [here](https://zachyoung.dev/posts/dataview-snippets#get-links-to-previous-and-next-daily-notes).

## Retrieve frontmatter from another note

Instead of reading the contents of the file and parsing it manually, it’s recommended to use `app.metadataCache.getFileCache` from the Obsidian public api.

In this example, we can check a note for a field in frontmatter called `count` in another note, and if it exists, increment it and set it in the current note’s frontmatter. Otherwise, fallback to setting it to “1”.

```
<%*
const file = tp.file.find_tfile("Note.md");
const fileCache = app.metadataCache.getFileCache(file);

let count = 1;
if (fileCache?.frontmatter?.count) {
  count = fileCache.frontmatter.count + 1;
}
-%>
---
count: <% count %>
---
```

A common use case for this is to get frontmatter from another note in the same folder as your current folder and increment it. Here’s an example of how to do that.

```
<%*
// Setup fallback if no other files in folder
let count = 1;

// Get all files in current folder that aren't this file
const filesInFolder = app.vault.getMarkdownFiles().filter(
  x => x.parent?.path === tp.file.folder() && x.path !== tp.file.path(true)
);

if (filesInFolder.length > 0) {
  // Sort files by path descending and get latest file's cache
  filesInFolder.sort((a, b) => a.path < b.path ? 1 : -1);
  const latestTFile = filesInFolder[0];
  const fileCache = app.metadataCache.getFileCache(latestTFile);

  // If latest file has count, increment it by 1 and use in new note's frontmatter
  if (fileCache?.frontmatter?.count) {
    count = fileCache.frontmatter.count + 1;
  }
}
-%>
---
count: <% count %>
---
```

## Update frontmatter

Instead of reading the contents of the file and parsing it manually, it’s recommended to use `app.fileManager.processFrontMatter` from the Obsidian public api.

You must mutate the `frontmatter` object directly, do not try to copy the object first and then do mutations.

```
<%*
const file = tp.file.find_tfile(tp.file.path(true));
await app.fileManager.processFrontMatter(file, (frontmatter) => {
  frontmatter["status"] = "In progress"; // create a new property or update an existing one by name
  frontmatter["review count"] += 1; // increment the value of an existing property by one
  delete frontmatter["ignored"]; // delete a property
});
-%>
```

If you want to use this API when creating a file, it won’t work because the file hasn’t been added to Obsidian’s metadata cache yet. You’ll need to wrap it in `tp.hooks.on_all_templates_executed` to wait for the file to finish being created, then update the newly created file’s frontmatter.

```
<%*
tp.hooks.on_all_templates_executed(async () => {
  const file = tp.file.find_tfile(tp.file.path(true));
  await app.fileManager.processFrontMatter(file, (frontmatter) => {
    frontmatter["status"] = "In progress";
    frontmatter["review count"] += 1;
    delete frontmatter["ignored"];
  });
});
-%>
```

## Run Obsidian frontmatter formatter on every file

If you want Obsidian to run it’s frontmatter formatter on every file in your vault, you can call `app.fileManager.processFrontmatter()` with an empty callback function to trigger the frontmatter formatter. I’ve included error handling, you can check the Obsidian dev console to see which files failed to format and why.

```
<%*
await Promise.all(
  app.vault.getMarkdownFiles().map(async file => {
    try {
      await app.fileManager.processFrontMatter(file, () => {});
    } catch (err) {
      console.error(`Failed to process ${file.path}\n\n${err}`);
    }
  })
);
-%>
```

You can also mutate frontmatter in every file with this method.

```
<%*
await Promise.all(
  app.vault.getMarkdownFiles().map(async file => {
    try {
      await app.fileManager.processFrontMatter(file, (frontmatter) => {
        if (!frontmatter["status"]) {
          frontmatter["status"] = "Unknown";
        }
        delete frontmatter["old status"];
      });
    } catch (err) {
      console.error(`Failed to process ${file.path}\n\n${err}`);
    }
  })
);
-%>
```

## Rename all files to match top level heading

We can loop over all the files in the vault and use `app.metadataCache.getFileCache()` to get the headings for each file, then use `app.fileManager.renameFile()` to rename the file. You can view the results of this script in the dev console (`CMD Shift I`).

```
<%*
// Loop over all markdown files in vault
await Promise.all(
  app.vault.getMarkdownFiles().map(async file => {
    try {
    // Get top level heading from file
      const { headings } = app.metadataCache.getFileCache(file);
      const topLevelHeading = headings?.find(h => h.level === 1);
      if (topLevelHeading) {
        // If a top level heading is found, rename file
        const oldPath = file.path;
        const newPath = tp.obsidian.normalizePath(`${file.parent.path}/${topLevelHeading.heading}.md`);
        await app.fileManager.renameFile(file, newPath);
        console.log(`Renamed ${oldPath} to ${file.path}`);
      } else {
        // If top level heading is not found, let user know in console
        console.warn(`Top level heading not found in ${file.path}`);
      }
    } catch (err) {
      // If any files fail to be renamed for any reason, let user know in console
      console.error(`Failed to process ${file.path}\n\n${err}`);
    }
  })
);
-%>
```

## Escaping frontmatter in a template

If you don’t want your frontmatter in your templates to be parsed by Obsidian or any Obsidian plugins (like Dataview), then you can escape your frontmatter by wrapping the starting and ending `---` with Templater tags to escape it.

```
<% "---" %>
date: <% tp.date.now() %>
key: value that is not frontmatter because the --- is escaped
<% "---" %>
// Rest of template below
```

## Escaping nested Templater tags

If you want the result of executing a Templater command to return a Templater command, you can escape the Templater tags with the `\` character.

```
// Does not work, will throw error
<% "<% tp.file.title %>" %>
// Does work, will return <% tp.file.title %>
<% "\<% tp.file.title %\>" %>
```

## Reuse value from prompt or suggester

Instead of prompting for the same value multiple times, you can prompt for it once, store it in a variable, and reference that variable multiple times.

Before:

```
<%* await tp.system.prompt("Result") %>
<%* await tp.system.prompt("Result") %>
```

After:

```
<%*
const result = await tp.system.prompt("Result")
_%>

<% result %>
<% result %>
```

## Find and replace content in another file

If you want to find and replace content in another file you can use `app.vault.process()`. The callback function to replace the data in the file must be synchronous.

```
<%*
// Prompt for data to replace
const currentVersion = await tp.system.prompt("In-game Version");
// Replace content with user provided data
const file = tp.file.find_tfile("file name");
await app.vault.process(file, (data) => {
  return data.replace(/releaseVersion = ".*"/gi, `releaseVersion = "${currentVersion}"`);
});
_%>
```

Alternatively, you can use `app.vault.read()` then `app.vault.modify()` to read then modify the contents of a file. This will allow you to perform asynchronous operations when doing replacements.

```
<%*
// Prompt for data to replace
const currentVersion = await tp.system.prompt("In-game Version");
// Get contents of file
const file = tp.file.find_tfile("file name");
const content = await app.vault.read(file);
// Replace content with user provided data
const newContent = content.replace(/releaseVersion = ".*"/gi, `releaseVersion = "${currentVersion}"`)
await app.vault.modify(file, newContent);
_%>
```

## Append content in another file

If you want to append content in another file you can use `app.vault.process()`. The callback function to replace the data in the file must be synchronous.

```
<%*
const file = tp.file.find_tfile("file name");
await app.vault.process(file, (data) => {
  // Append content (use \n for line break)
  return data + "\nContent you want to append";
});
_%>
```

Alternatively, you can use `app.vault.read()` then `app.vault.modify()` to read then modify the contents of a file. This will allow you to perform asynchronous operations when doing replacements.

```
<%*
// Get contents of file you want to edit
const file = tp.file.find_tfile("file name");
const content = await app.vault.read(file);
// Append content (use \n for line break)
const newContent = content + "\nContent you want to append";
// Update file you want to edit
await app.vault.modify(file, newContent);
_%>
```

## Append content to end of specific line in another file

If you want to append content to the end of a specific line in another file you can use `app.vault.process()`. The callback function to replace the data in the file must be synchronous.

```
<%*
const lineNumber = 3;

const file = tp.file.find_tfile("file name");
await app.vault.process(file, (data) => {
  // Split content into lines
  const lines = data.split("\n");
  // Append content to end of specific line
  lines[lineNumber] += "Content you want to append;
  // Join lines back together
  return lines.join("\n");
});
_%>
```

Alternatively, you can use `app.vault.read()` then `app.vault.modify()` to read then modify the contents of a file. This will allow you to perform asynchronous operations when doing replacements.

```
<%*
const lineNumber = 3;

// Get contents of file you want to edit
const file = tp.file.find_tfile("file name");
const content = await app.vault.read(file);
// Split content into lines
const lines = content.split("\n");
// Append content to end of specific line
lines[lineNumber] += "Content you want to append";
// Join lines back together
const newContent = lines.join("\n");
// Update file you want to edit
await app.vault.modify(file, newContent);
_%>
```

## Executing a command

You can get a list of the command IDs by opening the dev console (`CMD Shift I`) and typing `app.commands.commands` into the console.

This example will run the “Daily notes: Open today’s daily note” command.

```
<%*
app.commands.executeCommandById("daily-notes");
%>
```

## Copy content between headers at cursor

You can place your cursor in a section, run this script, and have everything between the heading before and the heading after copied to your clipboard.

```
<%*
const { line: cursorLine } = app.workspace.activeEditor.editor.getCursor();
const currentTFile = tp.file.find_tfile(tp.file.path(true));
const { headings, sections } = app.metadataCache.getFileCache(currentTFile);
let headingAboveSection;
let cursorSection;
let headingBelowSection;
for (const index in sections) {
  const section = sections[index];
  if (!cursorSection) {
    if (section.type === "heading") {
      headingAboveSection = section;
    }
    const { line: startLine } = section.position.start;
    const { line: endLine } = section.position.end;
    if (startLine <= cursorLine && cursorLine <= endLine) {
      cursorSection = section;
    }
  } else if (section.type === "heading") {
    headingBelowSection = section;
    break;
  }
}
const { line: startLine } = headingAboveSection.position.start;
const endLine = headingBelowSection?.position.start.line ?? editor.lastLine();
const sectionContent = tp.file.content.split("\n").slice(startLine, endLine).join("\n");
window.navigator.clipboard.writeText(sectionContent);
-%>
```

## Using Templater functions outside of Templater

You can use many of the Templater functions either in other plugins or in the developer tools console (`CMD Shift I`).

Here’s an example of using `tp.system.prompt`.

```
// Get "system" module
const systemModule = app.plugins.getPlugin('templater-obsidian').templater.functions_generator.internal_functions.modules_array.find(x => x.name === "system").static_object;

const value = await systemModule.prompt("What genre would you like to choose?");
```

And an example of using `tp.date.now`.

```
// Get "date" module
const dateModule = app.plugins.getPlugin('templater-obsidian').templater.functions_generator.internal_functions.modules_array.find(x => x.name === "date").static_object;

const now = dateModule.now("YYYY-MM-DD");
```

## HTML in Notice

You can have HTML in a Notice (in app notification) by appending HTML elements to `notice.noticeEl`.

```
<%*
const notice = new Notice();
notice.noticeEl.append(
  createEl("strong", { text: "Success" }),
  " script created 3 files",
);
-%>
```

If you need to set the duration of the Notice and have HTML, you can use an empty string for the base Notice text and still append HTML to it.

```
<%*
const notice = new Notice("", 5000);
notice.noticeEl.append(
  createEl("strong", { text: "Success" }),
  " script created 3 files",
);
-%>
```