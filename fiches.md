---
titre: fiches animation
---

# liste des fiches

<ul>
{% for fiche in site.fiches %}
	<li>
		<h2><a href="{{ fiche.url | relative_url }}">{{ fiche.titre }}</a></h2>
	</li>
{% endfor %}
</ul>
