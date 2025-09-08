+++
date = '{{ .Date }}'
title = '{{ replace (replaceRE  `^\d{4}-\d{2}-\d{2}-` "" .Name 1)  "-" " " | title }}'
tags = []
draft = true
+++