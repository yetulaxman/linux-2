
## Accessing biocontainer course material

https://yetulaxman.github.io/Biocontainer/

# Testing creating slides from markdown

Training material — slides, exercises, cheatsheets

## Online and class room versions

The same material should work for both. The only change between the
different course types are in the practicalities, in file
`_lectures/00-*`. Just swap that file to fit the type of the course
at hand, putting the other type's template into `attic` directory.


## Updating the course

Please, see [instructions for contributing](CONTRIBUTING.md) before
making this course even greater.


## Automatic web page and slide generation

Using static site generator Jekyll and github automation, too easy :)


### Known problems

#### Symbolic links

Jekyll does not like symbolic links, I guess. In the case of any
random looking errors, check for symbolic links and simply
remove them with

```bash
find _includes/slide-template -type l -delete_
```

#### Changing title field in the front matter while server is running

...is a problem (bug?), probably due to the automatic generation of
the lecture list...

#### Field 'date' in the front matter of collection source

If you do, the page is not written. No errors, no nothing.  Just don't
put 'date:' into the front matter. Crazy bug.


## Slideshow's inner workings

The layout <_layouts/slides-csc.html> adds minimal CSS
<css/majestic-csc.css> and Javascript <js/majestic.js> sugar on top of
the Jekyll generated HTML to make it into a very basic slideshow.


### How to use slides

- use arrow keys or click left/righ side to change slides.
- print slides in PDF format from browser (Chrome)

### How slides-csc JS works

- onload, split content into slides (DIVs) before H1, H2, and H3
  headings
- set the display style of the current slide to 'block', and 'none'
  for other slides
- detect events
    - window resize
        - content scaling
    - key press & mouse clicks
        - slide changes (arrow keys)
- that's all!


## Previewing site and slides locally before pushing to GitHub

GitHub Pages uses Jekyll site generator. It can be run locally to
preview site before pushing to GitHub.

Follow the instructions from
https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/

After having fresh ruby and bundler installed (in Linux just `sudo apt
install bundler`), it's basically

```
sudo apt-get install bundler
sudo apt-get install build-essential patch ruby-dev zlib1g-dev liblzma-dev
sudo gem install nokogiri

bundle install
bundle exec jekyll serve &
```

and open http://localhost:4000

or if running the whole thing in a virtual machine in cpouta, open the firewall
for 4040, and start the server with

```
bundle exec jekyll serve --host 0.0.0.0 --port 4040
```

and open `http://<ip-for-cpouta-vm>:4040` in your browser.









