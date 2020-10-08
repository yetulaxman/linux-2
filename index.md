---
title: WIP - Biocontainers(101) - Containerised Bioinformatics
author: CSC Training
---

# This is not really a course material, but a test

CSC training course for ...

To whom: You want to scale up from local workstations
or laptops to the CSC environment Puhti and Allas and
to use preinstalled software interactively or as batch
jobs.
After this course you should be able to choose the correct
resources for your jobs, location for files, and know where
to look for more information.

Practicalities for the [online course]({{ site.baseurl }}{% link
StudyGroupDec2020.md %}) and the regular [class room course at CSC's
premises]({{ site.baseurl }}{% link WhereAboutsCSCPremises.md %}).


# Course pre-requirements

Before this coursy, you should know the basics of linux command line
usage, e.g. having completed Linux-1, or equivalently,

- be familiar with basic Linux command line use and shell programming
- be familiar with some text editor such as Emacs, Vi or Nano
- have access to a Linux (virtual) machine, preferably with rights to
  install additional utility software
- have an account at CSC and access to a computing project

# Lectures

Lectures as HTML slides. Use cursor keys or click left/right side of
the slide to change it. You can print the slides to PDF with Chrome,
just tweak the print settings a bit.

{% for slide in site.lectures %}
- [{{ slide.title }}]({{ slide.url | relative_url }})
{% endfor %}


# Tutorials

{% for tutorial in site.tutorials %}
- [{{ tutorial.title }}]({{ tutorial.url | relative_url }})
{% endfor %}

# Excercises

{% for exercise in site.exercises %}
- [{{ exercise.title }}]({{ exercise.url | relative_url }})
{% endfor %}


# Extras

{% for extra in site.extras %}
- [{{ extra.title }}]({{ extra.url | relative_url }})
{% endfor %}
