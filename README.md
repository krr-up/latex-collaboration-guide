# Latex Collaboration Guide

We maintain styles, classes, bibliographies, and shared resources not available via the TeX distribution in repositories in our github organization.

## Using Submodules

Do not copy styles, classes, bibliographies, or shared resources into a git repository.
Use **Git submodules** within your paper repositories, instead.
Conceptually, this means that Git will maintain a files from another repository within a subdirectory of your own repository (we suggest `include/<name>`).

Git submodules are always **locked to a specific commit.**
In other words, a submodule **won’t automatically update itself,** even when pulling in the latest changes of your paper repository.
You’ll have to **manually update the bibliography** whenever necessary.

On another note, **never edit the content of the submodule directly,** that is, within `include/<name>`.
If you need modify the files in a submodule, clone it to a location outside of your paper repository and contribute from there.

Read more about Git submodules on the [GitHub Blog][github-blog-git-submodules] and the in [Pro Git book][pro-git-book-git-submodules].

### Using Submodules with LaTeX

By default, the LaTeX toolchain won’t attempt to look for styles, classes, and bibliographies in subfolders.
If you build your paper with [`latexmk`][latexmk] (we can only recommend that), this can be easily solved.
Simply copy the [`.latexmkrc`][.latexmkrc] file to the top level of your repository,
and LaTeX will automatically find any styles, classes, and bibliographies in the `include` folder if you run with `latexmk`:
```sh
$ latexmk -pdf paper.tex
```

If you do not use `latexmk`, you need to set the following environment variables in your `.bashrc` or LaTeX IDE instead:
```sh
export BIBINPUTS="./include//:"
export BSTINPUTS="./include//:"
export TEXINPUTS="./include//:"
```

## Adding Submodules to your Paper Repositories

**Add the repository as a submodule** to your paper repository as follows:
```sh
$ git submodule add ../<name> include/<name>
```
(Replace `../<name>` with `https://github.com/krr-up/<name>` if your repository is hosted outside of the [`krr-up` organization][krr-up].)

Next, **commit and push** the changes to make the submodule available to your coworkers.
After your coworkers have pulled the change, they will need to **execute the following command:**
```sh
$ git submodule update --init --recursive
```

The submodule will now stay at the latest state of the `master` branch **at the time of adding it** unless manually updated later on.

### Updating Submodules in your Paper Repositories

You can **bring the latest changes to your paper repository** as follows:

```sh
$ cd include/bibliography
$ git fetch
$ git reset --hard origin/master
$ cd ../..
$ git add include/bibliography
$ git commit -m "Update bibliography"
$ git push
```

Finally, keep in mind that you need to run
```sh
$ git submodule update --init --recursive
```
**every time one of the following happens:**
- You **clone** your paper repository from scratch.
- Someone else **updates the submodule** to to a newer version.
- Files in `include/<name>` are **missing** or LaTeX complains about them.
- `git status` tells you `modified: include/bibliography (new commits)` and you didn’t expect this.
- Someone else adds a **new submodule** to your paper repository.

## Available Submodules

- https://github.com/krr-up/bibliography
- https://github.com/krr-up/latex-style-comments

For licensing reasons, we set the visibility of the following repositories to private:
- https://github.com/krr-up/latex-class-llncs
- https://github.com/krr-up/latex-class-eptcs
- https://github.com/krr-up/latex-style-aaai
- https://github.com/krr-up/latex-class-tlp

[.latexmkrc]: .latexmkrc
[krr-up]: https://github.com/krr-up
[latexmk]: https://mg.readthedocs.io/latexmk.html
[github-blog-git-submodules]: https://github.blog/2016-02-01-working-with-submodules/
[pro-git-book-git-submodules]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
