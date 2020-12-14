+++
title = "{{ replace .Name "-" " " | title }}"
date = "{{ .Date }}"
# description = "Optional page description."
tags = [{{ range $plural, $terms := .Site.Taxonomies }}{{ range $term, $val := $terms }}"{{ printf "%s" $term }}",{{ end }}{{ end }}]
+++

Blog post!
