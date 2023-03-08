---
layout: post
title:  How To Merge Jupyter Notebooks in Git Repo Without a Headache?
categories: machine-learning jupyter notebooks repo
date:   2022-01-01 20:34:00 +0200
author: Konrad
---


## How To Merge Jupyter Notebooks in Git Repo Without a Headache?

Have you ever felt this faster heart beating, when you had to merge one git branch to another, and it turned out, that there are diffs in Jupyter Notebooks? Mike did.

![][image-1]

Let me show you one tool today that will make your life easier. It only takes 1 minute to avoid having to look at images stored in base64 in JSON files (it made me weak already…).

We’ll start smoothly, with just comparing notebooks, then we’ll move to GUI version, where we can compare files in a browser, and finally, we’ll fire the final firecracker — notebook merge.

Feel invited!

*TL;DR at the bottom of the post 😉*

### What’s in store for us? — table of contents
1. Mike has had enough — what’s the problem?
2. Conflicts in Jupyter Notebooks
3. The rescue
4. Comparing notebooks
5. Merging notebooks
6. Git integration
7. Plugin for Jupyter
8. A treat at the end
9. Links
10. TL:DR;

## Mike has had enough — what’s the problem?

Mike’s company uses GIT. Every self-respecting team uses some kind of version control system because they don’t move their code on a memory stick, right?

And as you know, in GIT we have branches, and when several people work on a project, the branches need to be connected, i.e. merged. And Mike is fluent in using GIT like a crawfish.

In Mike’s project, they have a python code in a very nice structure created by [Data Science Cookie Cutter][1]. As in every Data Science project, there is a folder called notebooks/, and inside that folder nothing else but some Jupyter Notebooks.

Jupyter Notebooks are stored in JSON format. We have some text files, so it seems that the GIT repo is a good place for this kind of document.

Mike has done some fixes in his python file. Standard procedure after a patch. Commit. Push. Pull Request.

But after the commit, he remembered that he created his branch a long time ago, so he merged the changes from the develop branch, to make the pull (merge — for GitLab users) request work nicely.

## Conflicts in Jupyter Notebooks

Someone introduced changes to Jupyter Notebook, which Roman just tweaked a bit. While trying to merge repository branches, conflicts appeared.
Roman fired up git diff. What he saw was a forest… well, text like in the picture below.

<div style="width: 100%; height: 0px; position: relative; padding-bottom: 56.875%;"><iframe src="https://streamable.com/e/p076fw" frameborder="0" width="100%" height="100%" allowfullscreen style="width: 100%; height: 100%; position: absolute;"></iframe></div>

It’s not good. In fact, it’s not obvious what’s going on. So, how to choose which changes should be kept and which should not?

Let’s check via some IDE or some GIT client like Sublime Merge

![][image-2]

A little better, but still not good. Digging around in JSON while merging notebooks is not the most pleasant thing to do…

So, what now? Does Mike stand a chance against the brutal Json mutated by Git?

## Nbdime to the rescue!

Nbdime is a tool to compare and merge changes in Jupyter Notebooks.

[https://nbdime.readthedocs.io/en/latest/][2]

Mike went straight to installing the tool, which, as in Python, is pretty standard:

pip install nbdime

Now what? What can Mike do next with the tool he installed?

## Comparing notebooks

The first thing Mike checked is comparing notebooks. And here we have two possibilities.

### From the terminal level

If we don’t have access to the graphical environment (because we are connected to a remote server via SSH), we can use the comparison displayed in the terminal. This is done with the command

nbdiff notebook\_1.ipynb notebook\_2.ipynb

Then we get something similar to:

![Diff notebooks in the console][image-3]

Do you admit that it is better? We know that two cells were added, that one modified, and that one output was deleted. Yes, the same can be read from JSON, but I guess it’s easier from the text above, right?

But it doesn’t end there because nbdiff didn’t say the last word. Let’s move on to…

### GUI in the browser

If we have a graphical environment (because we are on our computer or a remote machine connected via RDP), we can use the GUI. This will launch a page in the browser, where we can conveniently compare two notebooks.

And here Mike’s eyes lit up as bright as the headlights on his Toyota Prius. He issued a command like the following:

nbdiff-web \[~<commit>~ \[~<commit>~]] \[~<path>~]

And after a while, the browser launched a notebook comparison he hadn’t even dreamed of!

![Notebook diff in nbdime — nbdiff-web][image-4]

Isn’t it beautiful?

- Okay, I can compare two notebooks. But what about my conflicts in git? — Mike rightly pointed out.
- Will nbdime help me?

## Merging notebooks

Nbdime stands for “Jupyter **N**ote**b**ooks **D**iffing and **M**erging”. As we can guess, apart from seeing the changes (diff), we can also merge them!

Nbdime gives us the ability to merge notebooks, that is, to combine changes that have been made to the same file by two different\* people.

And here is what Mike likes the most. Here, Mike’s eyes glazed over with emotion. No more merging changes in JSON or combining them with simultaneous notebooks. No more agonizing, no more moaning. Pure excitement at the simplicity of combining two notebooks into one. Just as two people on the wedding cake are now one, two notebooks that don’t quite fit together are now one…

- Show me the code! — Shouted the colleague to whom Mike wanted to show off. In the terminal, he issued the command below and muttered “ENTER”.

nbmerge-web base.ipynb local.ipynb remote.ipynb --out merged.ipynb

And to their eyes appeared a screen, as if from a dandelion:

![Merge notebooks in the browser][image-5]

With a few efficient clicks, Mike merged the changes that were occurring in the notebook. These included:

* Changes to the Metadata, such as the tags for a particular cell (YES, we can merge these changes too! Isn’t it beautiful?)

![][image-6]

* Changes that have occurred to a given cell in one of the notebooks:

![][image-7]

* Changes that occurred in a cell in **both** notebooks:

![][image-8]

* Deleted cells:

![][image-9]

* Added cells:

![][image-10]

**nbmerge** can use different strategies for pre-merging changes. Below, I’ve pasted the parameters you can give it to get the merge party started:

```bash
➜ nbmerge-web
usage: nbmerge-web \[-h] \[--version] \[--config]
\[--log-level {DEBUG,INFO,WARN,ERROR,CRITICAL}] \[-s] \[-o]
\[-a] \[-m] \[-d]
\[--merge-strategy {inline,use-base,use-local,use-remote}]
\[--input-strategy {inline,use-base,use-local,use-remote}]
\[--output-strategy {inline,use-base,use-local,use-remote,remove,clear-all}]
\[--no-ignore-transients] \[-p PORT] \[-b BROWSER] \[--persist]
\[--ip IP] \[-w WORKDIRECTORY] \[--base-url BASE\_URL]
\[--show-unchanged] \[--out OUT]
base local remote
```

## Git integration

And here is the moment that for Mike was a saviour. **Nbdime integrates with Git**. And this is the feature everyone was waiting for.

### Diff

Nbdime can be activated per repository you work in, or globally, for all repositories and handling **all** diff

Roman doesn’t grind in the dance, so he activated globally. YOLO.

nbdime config-git --enable --global

If he wasn’t such a wuss, he could have omitted the --global parameter, and then it would only activate for the repository it was currently in.

### Merge

Here the situation is a bit more complicated. If we set **nbmerge** as the default mergetool in git, it will handle **all files**. **We rather don’t want this to happen**.

The most convenient way here, in case of conflicts, is to use the command below. This will resolve conflicts using nbdime in this particular notebook, and in code files in our primary tool (VSCode, Pycharm, VIM, whateva).

git mergetool --tool=nbdime my\_awesome\_notebook.ipynb

>  Take a look at the documentation: [https://nbdime.readthedocs.io/en/latest/vcs.html#git-integration][3].
>  There you will find detailed description how to register nbdime as mergetool globally, selectively, etc.

*P.S. Apparently it also works with Mercurial, but does anyone else use it?*

## Plugin for Jupyter

To top it all off, Mike discovered that **nbdime has a plugin for Jupyter Lab**! He didn’t even have to reach into the console any more (although he liked it a lot). He could preview notebook changes directly from the Jupyter Notebooks IDE!

![Diff notebooks in Jupyter Lab][image-11]

Here’s a link to the extension and installation details: [https://nbdime.readthedocs.io/en/latest/extensions.html][4]

Basically, just issue this command:

nbdime extensions --enable \[--sys-prefix/--user/--system]

## A treat at the end — nbshow

Things happen in life, sometimes the only thing we have available is a terminal. We have connected to a remote machine via SSH. We want to view a notebook. Now what? Again, watching JSONs that don’t speak much?

This is where **nbshow** comes in handy.

An example nbshow output might look like this:

```bash
➜ nbshow prep\_data/image\_data\_guide/03c\_pytorch\_preprocessing.ipynb
notebook format: 4.4
metadata (known keys):
kernelspec:
display\_name: conda\_pytorch\_latest\_p36
language: python
name: conda\_pytorch\_latest\_p36

\[...]

code cell 25:
execution\_count: 10
source:
sample = iter(sample)
sample\_augmented = iter(sample\_augmented)
markdown cell 26:
source:
Re-rull the cell below to sample another image
code cell 27:
execution\_count: 11
source:
fig, ax = plt.subplots(1, 2, figsize=(10,5))
image = next(iter(sample))\[0]
image\_augmented = next(iter(sample\_augmented))\[0]

ax\[0].imshow(image.permute(1, 2, 0))
ax\[0].axis('off')
ax\[0].set\_title(f'Before - {tuple(image.shape)}')
ax\[1].imshow(image\_augmented.permute(1, 2, 0))
ax\[1].axis('off')
ax\[1].set\_title(f'After - {tuple(image\_augmented.shape)}');
plt.tight\_layout()
outputs:
output 0:
output\_type: display\_data
data:
image/png: iVBORw0K...<snip base64, md5=3ea6c670233dfdcd...>
text/plain: <Figure size 720x360 with 2 Axes>
    metadata (unknown keys):
    needs_background: light
```

This may not be as friendly as the view in the browser, but in a sub-critical situation it’s certainly better than JSON 😉

## Links

* [Nbdime][5]

* [Data Science Cookie Cutter][6]

* [Sublime Merge][7]

## TL;DR:

```bash
$ pip install nbdime

$ nbdiff notebook\_1.ipynb notebook\_2.ipynb

$ nbdiff-web \[~<commit>~ \[~<commit>~]] \[~<path>~]

$ nbmerge-web base.ipynb local.ipynb remote.ipynb --out merged.ipynb

$ nbdime config-git --enable --global

$ git mergetool --tool=nbdime biutiful\_notebook.ipynb
```

[1]:	https://drivendata.github.io/cookiecutter-data-science/#directory-structure
[2]:	https://nbdime.readthedocs.io/en/latest/
[3]:	https://nbdime.readthedocs.io/en/latest/vcs.html#git-integration
[4]:	https://nbdime.readthedocs.io/en/latest/extensions.html
[5]:	https://nbdime.readthedocs.io/en/latest/index.html
[6]:	https://drivendata.github.io/cookiecutter-data-science/#directory-structure
[7]:	https://www.sublimemerge.com/

[image-1]:	/assets/img/posts/1*zR0QF6gKUlQkvWGRb5KxAg.jpeg
[image-2]:	/assets/img/posts/1*HuZof7bSLZbWmOaDKrivDw.png
[image-3]:	/assets/img/posts/1*dQ3RGvWCeYuvTd5-Q3R-qQ.png
[image-4]:	/assets/img/posts/1*PU5feR0HqaVEqA-Rn2f5qQ.png
[image-5]:	/assets/img/posts/1*G6Q9U758Yd9vVmZ8ff67tA.png
[image-6]:	/assets/img/posts/1*PLRNw9UczCUJMGpIh1fdyw.png
[image-7]:	/assets/img/posts/1*8CbVS5T5M7c99gTCHRk08A.png
[image-8]:	/assets/img/posts/1*KQWXcO2MUR-uHn6zUM7vww.png
[image-9]:	/assets/img/posts/1*-FV1NnORtUA17DyZZw0YZg.png
[image-10]:	/assets/img/posts/1*_lUk1B-phnNOM6QR7hoEzQ.png
[image-11]:	/assets/img/posts/1*ayW5nfEdjmRm5WwN5xI8sQ.png