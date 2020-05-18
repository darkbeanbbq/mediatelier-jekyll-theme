---
titre: tutoriels
---

[liste de tous les tutos](liste-tutos.html)

# liste des tutos

<ul>
    {% for tutoriel in site.tutoriels %}
      <li>
        <h2>
          <a href="{{ tutoriel.url | prepend: site.baseurl }}">{{ tutoriel.titre }}</a>
        </h2>
      </li>

      <img  src="{{ site.baseurl }}/tutoriels/images/{{ tutoriel.image-titre }}">

      {{ tutoriel.excerpt }}
      <a class="btn btn-default" href="{{ tutoriel.url | prepend: site.baseurl }}">vers tuto</a>
    {% endfor %}
  </ul>
