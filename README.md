# repository of linux configs

The main idea is to have config files versioned where they are, without doing any copy operations.

contains stuff like
- bashrc
- zshrc
- git config

idea from this beautiful GPN talk: https://www.youtube.com/watch?v=g-TlsNUx0RQ&t=2280

## Setup

add alias for easier modification:
```alias config ='/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME'```
(set custom .git folder to not conflict with any existing ~/.git folders)

then clone the repo
```git init --bare $HOME/.dotfiles```

make `git status` stop showing untracked files (we only want to add a very small subset of files under $HOME)
```config config --local status.showUntrackedFiles no```

add/update files from anywhere:
```config add [filename]```

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
```config update-index --no-assume-unchanged README.md```
