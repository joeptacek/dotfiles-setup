# dotfiles setup

track dotfiles with git

## initial setup

initialize a bare repo in `$HOME`

```bash
git init --bare "${HOME}/.dotfiles.git"
```

grab `gitignore` from this repo, write to `$HOME/.gitignore`

```bash
[ ! -f "${HOME}/.gitignore" ] && curl -fsS https://raw.githubusercontent.com/joeptacek/dotfiles-setup/master/gitignore > "${HOME}/.gitignore"
```

set temporary alias `dotfiles-git` for using git to work with dotfiles in `$HOME`

```bash
alias dotfiles-git='git --git-dir="${HOME}/.dotfiles.git" --work-tree="${HOME}"'
```

or permanently add this alias to `.bashrc`
```bash
echo "alias dotfiles-git='git --git-dir="${HOME}/.dotfiles.git" --work-tree="${HOME}"'" >> "${HOME}/.bashrc" && source "${HOME}/.bashrc"
```

to track a new file, un-ignore it from `.gitignore` and then use `dotfiles-git add` to stage it

```bash
echo "!.bashrc" >> "${HOME}/.gitignore"
dotfiles-git add .bashrc
```

after committing, push to your remote

```bash
dotfiles-git remote add origin <url>
git push -u origin master
```

## cloning from remote

e.g., setting up an existing dotfiles repo on another machine

### manually

backup or remove any dotfiles in `$HOME` that would otherwise be duplicated

clone from `<url>` to a bare repo in `$HOME`

```bash
git clone --bare <url> "${HOME}/.dotfiles.git"
```
move all dotfiles from master branch into `$HOME`

```bash
git --git-dir="${HOME}/.dotfiles.git" --work-tree="${HOME}" checkout master .
```

### bash script

this will clone to a bare repo in `$HOME`, move duplicates to `$HOME/.dotfiles.bk`, and then move dotfiles from master branch into `$HOME`

at the end of this command, make sure to replace `<url>` with the remote URL

```bash
bash <(curl -fsS https://raw.githubusercontent.com/joeptacek/dotfiles-setup/master/dotfiles-install) <url>
```

## notes

instead of explicitly ignoring non-dotfiles with `.gitignore`, you can just have git keep quiet about untracked files; remove everything from `.gitignore` except `.dotfiles.git` itself

```bash
echo ".dotfiles.git" > "${HOME}/.gitignore"
dotfiles-git config --local status.showUntrackedFiles no
```

## see also

* [hacker news thread](https://news.ycombinator.com/item?id=11070797)
* [atlassian blog post](https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/)
