---
permalink: /maps/
title: "Maps "
excerpt: "Map placeholder"
author_profile: false
last_modified_at: 2025-01-06

gallery2:
  - url: /assets/images/_Map_all_bgc_floats_no_legend.png
    image_path: /assets/images/_Map_all_bgc_floats_no_legend.png
    alt: "placeholder image 3"
    title: "Image 3 title caption"
---

![image-center](/assets/images/_Map_all_bgc_floats_no_legend.png){: .align-center}

This post is for troubleshooting why my images are not rendering on the Minimal Mistakes site. 

The minimal mistakes jekyll [assets/images](https://github.com/mmistakes/minimal-mistakes/tree/master/docs/assets/images) folder images are all either .png or .jpg files

The [post](https://mmistakes.github.io/minimal-mistakes/post%20formats/post-image-standard/) & [file](https://github.com/mmistakes/minimal-mistakes/blob/master/docs/_posts/2010-08-05-post-image-standard.md) on including a standard image says:

The preferred way of using images is placing them in the `/assets/images/` directory and referencing them with an absolute path. Prepending the filename with `{% raw %}{{ site.url }}{{ site.baseurl }}/assets/images/{% endraw %}` will make sure your images display properly in feeds and such.

In my `config.yml`file I added the lines:

```yml
url		 : "sarahtanja.github.io"
baseurl	 : "/lab-book"
```



**HTML:**

```html
{% raw %}<img src="{{ site.url }}{{ site.baseurl }}/assets/images/_Map_all_bgc_floats_no_legend.png" alt="">{% endraw %}
```
{% raw %}<img src="{{ site.url }}{{ site.baseurl }}/assets/images/_Map_all_bgc_floats_no_legend.png" alt="">{% endraw %}

... nothing 

**Kramdown:**

```markdown
{% raw %}![alt]({{ site.url }}{{ site.baseurl }}/assets/images/_Map_all_bgc_floats_no_legend.png){% endraw %}
```
{% raw %}

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/_Map_all_bgc_floats_no_legend.png)

{% endraw %}

... 

**Kramdown without '{% raw %} {% endraw %} wrap':**

```kramdown
![img-png]({{ site.url }}{{ site.baseurl }}/assets/images/_Map_all_bgc_floats_no_legend.png)
```

![img-png]({{ site.url }}{{ site.baseurl }}/assets/images/_Map_all_bgc_floats_no_legend.png)

{% include gallery id="gallery2"  caption="This is a sample gallery with **Markdown support**." %}

![img-jpg]({{ site.url }}{{ site.baseurl }}/assets/images/_Map_all_bgc_floats_no_legend.png)
