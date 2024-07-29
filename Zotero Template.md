<%"---"%>

category:LiteratureNote
tags: 📥️/📜️/🟥️
publish: true

aliases: 
  - {{title | replace(":", "") | replace("#", "") | replace("^", "") | replace("|", "") | replace("\[", "") | replace("\]", "") | replace("\\", "") | replace("/", "")}}

  - {{citekey}}
<%"---"%>

{% persist "readStatus" %}
{% if isFirstImport %}
- [U] Read Status
{% else %}
{% endif %}
{% endpersist %}


{# Rest of your existing template #}

> zotero_link:: {{pdfZoteroLink}}

## Cite
> citekey:: {{citekey}}

## Keywords
> keywords:: {{allTags}}

## Authors
> authors:: {{authors}}{{directors}}

## Meta

{% for type, creators in creators | groupby("creatorType") -%}

{%- for creator in creators -%}

> **{{"First" if loop.first}}{{type | capitalize}}**::

{%- if creator.name %} {{creator.name}}  

{%- else %} {{creator.lastName}}, {{creator.firstName}}  

{%- endif %}  

{% endfor %}~ 

{%- endfor %}    

> **Title**:: {{title}}  

> **Year**:: {{date | format("YYYY")}}   

> **Citekey**:: {{citekey}} {%- if itemType %}  

> **itemType**:: {{itemType}}{%- endif %}{%- if itemType == "journalArticle" %}  

> **Journal**:: *{{publicationTitle}}* {%- endif %}{%- if volume %}  

> **Volume**:: {{volume}} {%- endif %}{%- if issue %}  

> **Issue**:: {{issue}} {%- endif %}{%- if itemType == "bookSection" %}  

> **Book**:: {{publicationTitle}} {%- endif %}{%- if publisher %}  

> **Publisher**:: {{publisher}} {%- endif %}{%- if place %}  

> **Location**:: {{place}} {%- endif %}{%- if pages %}   

> **Pages**:: {{pages}} {%- endif %}{%- if DOI %}  

> **DOI**:: {{DOI}} {%- endif %}{%- if ISBN %}  

> **ISBN**:: {{ISBN}} {%- endif %}{%- if url %} 
> 
> **URL**:: {{url}} {%- endif %}

## Related

{% for relation in relations -%}

{%- if relation.citekey -%}

> related:: {{relation.citekey}}

{% endif -%}

{%- endfor %}

## Attachments

> {%for attachment in attachments | filterby("path", "endswith", ".pdf") %}
>  [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}})  {% endfor %}.
{%- set regExpSummary = r/^#Summary/ -%}
{%- set regExpHypothesis = r/^#Hypothesis/ -%}
{%- set regExpMethodology = r/^#Methodology/ -%}
{%- set regExpResults = r/^#Results/ -%}
{%- set regExpSignificance = r/^#Significance/ %}

## Notes

{% if markdownNotes -%}
{% for n in notes %}
{%- if regExpSummary.test(n.note) %}

### Summary

{{n.note}}
{% elif regExpHypothesis.test(n.note) %}

### Hypothesis

{{n.note}}
  {% elif regExpMethodology.test(n.note) %}
  
### Methodology

{{n.note}}
  {% elif regExpResults.test(n.note) %}
  
### Results

{{n.note}}
  {% elif regExpSignificance.test(n.note) -%}
  
### Significance

{{n.note}}
  {% endif %}
{% endfor %}
{% endif %}

## Abstract

{% if abstractNote  %}
{{abstractNote}}
{% endif  -%}

## Annotation

{%  persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}
{% if newAnnotations.length > 0 %} 
###### Imported: {{importDate | format("YYYY-MM-DD HH:mm")}}

{% for annotation in newAnnotations %} 
{%- if annotation.annotatedText -%} - <mark class="hltr-{{annotation.colorCategory | lower}}">"{{annotation.annotatedText | escape}}”</mark> [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}) {%- endif %} 
{%- if annotation.imageRelativePath -%} ![[{{annotation.imageRelativePath}}]] [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}) {%- endif %}
{% if annotation.comment %}  
- {{annotation.comment}} [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}}) 
{% endif %} 
{% endfor %} 
{% endif %} 
{% endpersist %}
