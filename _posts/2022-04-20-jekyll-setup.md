---
title: "Installing Jekyll for Minimal Mistakes (Windows) "
date: 2022-04-20T03:30:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - VPS
---

1. Install Ruby


2. Install required gems by first navigating to directory with `Gemfile`.

	```bash
	cd project_dir
	bundle
	```

3. For building:

	```bash
	bundle exec jekyll build
	```

4. For building and serving:

	```bash
	bundle add webrick
	bundle exec jekyll serve
	```

5. For rebuilding upon changes:

	```bash
	bundle exec jekyll build --watch
	```





