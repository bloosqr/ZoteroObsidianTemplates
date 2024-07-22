# ZoteroObsidianTemplates
Some templates for integrating Zotero, AI and Obsidian

To get this to work 

(1) I would install the following plugins

  Highlightr         : This makes the Zotero highlight colors work properly instead of all turning to yellow.  
  Pandoc             : These make citations and citation pop ups all work nicely
  Pandoc Reference List 
  Tasks              : This allows you to track what papers to read
  Templater          : I am not sure you need this for this but you may because of all the dependencies ( will double check later)
  Zotero Integration : the key Template that makes all this work.

(2) Place the Zotero Template.md file in your fault and point your Zotero Integration plugin at it 
    This is under the Zotero Integration plugin Settings -> Template File
(3) Under Settings -> Appearance -> CSS Snippets 
    add the zotero-read-status.css file
What this does is add the text to the right of the "Read Status" in each .md file
(4) Think about how you want you want to create your summary sections as notes in Zotero. I am using PDFPals for this
The only thing that is important here is there are notes that have headers that start with

  #Summary
  For the summary of the paper
  #Hypothesis
  For the hypothesis
  #Methodology
  For the methodology
  #Result
  For the main results of the paper
  #Significance

You don't need all these fields, whether you have one or more. The template will fill in the fields for you in the .md file as you import.

 (5) save the Literature Summary Script.md somewhere in your vault. This will pull the data for you and put it in a table.

By default this script puts all the papers in a folder called "Literature" you should be able to edit the files to change this to something else if you'd like. 














