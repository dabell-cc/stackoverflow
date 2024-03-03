Answering [Use dark mode equivalent of Github Pages default theme?](https://stackoverflow.com/questions/70538816/use-dark-mode-equivalent-of-github-pages-default-theme)
Quick disclaimer, I'm brand new to github pages, jekyll, minima, the whole thing. I just didn't want my page to be ALWAYS light-mode, and frankly think everything should be polite enough to switch with the system mode setting of the device you're using - thankfully is IS possible without too much fuss. 

### TL;DR:
You can set the Minima theme to a dark skin (or one that adapts to the system setting). This is not supported by the version of the minima theme (2.5.1) used on github-pages currently but is available in the (as yet - 2024) unreleased minima 3. 
```
# In _config.yml
# theme: minima
remote_theme: jekyll/minima
plugins:
  - jekyll-remote-theme
minima:
  skin: dark # or auto if you want it to switch with your system settings.

```
```html
<!-- in 404.html -->
---
permalink: /404.html
layout: base 
---
<!-- Rest of the file here -->
```

I have to confess I found the github instruction pages a bit like a 'choose-your-own-adventure' book where you could go down a wrong path and then just end up confused. I've tried to explain a little of the steps I ended up following below.

### So, starting with a brand new github personal page in a repo:  

- Following github's instructions for creating a jekyll site - install jekyll, bundler, ruby, etc. 
- Run create an empty site in the local repo dir. 
```
jekyll new --skip-bundle .
```
- Edit the Gemfile to use the github-pages gem version. (Whatever that means, it's currently 231).
```
# gem "jekyll", "~> 4.3.3"
gem "github-pages", "~> 231", group: :jekyll_plugins
```
- Run `bundle install`... fix some errors of some things not being installed (`bundle add webrick` and `bundle add unf` were necessary for me).
```
bundle add unf
bundle add webrick
bundle install
```
- Once that's all working without errors you could push this to your github-pages repo or try to run it locally to see changes live with `bundle exec jekyll serve`. 
```
bundle exec jekyll serve
```

### Okay, now that that's working, let's do the thing you were asking in the question:
- Edit the `_config.yml` to comment out the local theme
```
# theme: minima
```
- Edit the `_config.yml` to add a remote theme
```
remote_theme: jekyll/minima

plugins:
  # whatever else was already here...
  - jekyll-remote-theme
```
- Edit the `_config.yml` to configure minima's settings
```
minima:
  skin: auto # if you want it to switch with your system settings.
```
- Update the default `404.html` file because [minima 3 changed 'default' to 'base'](https://github.com/jekyll/minima#base-layout)
```html
<!-- 404.html -->
---
permalink: /404.html
layout: base 
---
<!-- Rest of the file here -->
```
### References:
- [ Improve documentation for config options #760](https://github.com/jekyll/minima/pull/760) - I found this the most helpful while figuring this out.
- [How to change minima skin](https://github.com/jekyll/minima/issues/584) and [Skins does not work](https://github.com/jekyll/minima/issues/543) - made it clear that the old version (2.5.1) of minima used by github-pages was the problem.
- [minima 3 changed 'default' to 'base'](https://github.com/jekyll/minima#base-layout) - I happened to catch the name change, which helped later when I saw the error during a local build.
- [Minima skins available in v3](https://github.com/jekyll/minima#skins) - this explains what you can use now that you're pulling the latest minima.

