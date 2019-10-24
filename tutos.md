---
layout: tutoriel
titre: tutoriels
---

# liste des tutos
<ul>
{% for tutoriel in site.tutoriels %}
	<li>
		<h2>{{tutoriel.titre}}</h2>
	</li>
{% endfor %}
</ul>
