---
titre: tutoriels
---

# liste des tutos
<ul>
{% for tutoriel in site.tutoriels %}
	<li>
		<h2><a href="{{ tutoriel.url | relative_url }}">{{ tutoriel.titre }}</a></h2>
	</li>
{% endfor %}
</ul>
