# repository of linux configs

The main idea is to have config files versioned where they are, without doing any copy operations. After performing a `bare` clone of this repository, you can safely view what the remote has to offer and manually decide which files you want to have.

contains stuff like
- bashrc
- zshrc
- git config (contains aliases for gitk and tig)

idea was taken from this beautiful GPN talk: https://www.youtube.com/watch?v=g-TlsNUx0RQ&t=2280

## Setup

1) add alias for easier handling:
```
alias config ='/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
```

(set custom .git folder to not conflict with any existing ~/.git folders)

2) clone the repo
```
git clone --bare $HOME/.dotfiles
```

3) make `git status` stop showing untracked files (which is probably the majority under $HOME)
```
config config --local status.showUntrackedFiles no
```

After cloning on a "dirty" machine, files of the repository will be in the staging aread in status `deleted`. So first, do `config reset`, now you can watch the acutal differences with the local machine via `config diff`. Handling of diffs is a little funky, compared to usual git workflows. E.g. fetching changes from the remote will cause diffs that look like you locally removed what the remote added. Therefore: **before fetching, make sure you have no local changes!** But since you're the only person working on it, that shouldn't be much of an issue.

## ignoring files you don't want

E.g. having this README lying around in $HOME feels wrong. But a repo without a README is also bad.
So, to get rid of it after cloning:
- delete the file locally
- "assume unchanged".

Like so:
```
rm README.md
config update-index --assume-unchanged README.md
```

the .gitconfig provides the alias for that: `hellban <file>`

to watch the file again
```
config update-index --no-assume-unchanged README.md
```
