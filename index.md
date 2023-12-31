---
title: Instructions hébergées en ligne
permalink: index.html
layout: home
---

# Exercices pour l’analyste des données

Ces exercices soutiennent le cours Microsoft [DP-500 : conception et implémentation de solutions d’analytique à l’échelle de l’entreprise avec Microsoft Azure et Microsoft Power BI](https://docs.microsoft.com/training/courses/dp-500t00)

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/labs'" %}
| Module ILT | Laboratoire |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

<!--

## Demos

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Module | Demo |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
 
-->
