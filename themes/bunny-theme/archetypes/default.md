+++
title = "{{ replace .Name "-" " " | title }}"
date = "{{ .Date }}"
# If set, this will show up in the top navbar
menu = "main"
# description = "Optional page description."
# tags = [{{ range $plural, $terms := .Site.Taxonomies }}{{ range $term, $val := $terms }}"{{ printf "%s" $term }}",{{ end }}{{ end }}]
+++

Default page!
