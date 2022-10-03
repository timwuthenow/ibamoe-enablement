# Learn business automation

Access the labs at: https://timwuthenow.github.io

---

## Contributing to this project

The training labs are based on mkdocs.

Env setup:

~~~bin/bash
  pip install mkdocs
  pip install -e mkdocs-material
  pip install mkdocs-awesome-pages-plugin
  pip install mkdocs-enumerate-headings-plugin
~~~

Development:

After cloning this repo and during development check the changes. It will start the live-reloading docs server.

~~~bash
mkdocs serve
~~~

Publish:

~~~bin
mkdocs gh-deploy --force
~~~
