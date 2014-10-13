dotfiles
========

[sloria's dotfiles](https://github.com/sloria/dotfiles-old), rewritten as Ansible roles. Fully supports MacOSX. Red Hat and Debian support is good but not as complete.

a few neat features
-------------------

- zsh configured with [prezto](https://github.com/sorin-ionescu/prezto).
- nice fonts for the terminal and coding.
- iterm2 themes.
- anaconda python (Miniconda 3 distribution).
- a tmux.conf that's pretty neat.
- vim with [vim-plug](https://github.com/junegunn/vim-plug) for plugin management. All configuration in a single file [.vimrc](https://github.com/sloria/dotfiles/blob/master/roles/vim/files/vimrc).
- pluggable. Everything is optional. Fork this. Remove what you don't use. Configure what you do use.
- Mac packages installed with [homebrew][]. Mac apps installed with [homebrew-cask][].
- support for deploying to remote environments.

prerequisites (install these first)
-----------------------------------

- ansible >= 1.6
- homebrew (If on Mac OSX)
- git (homebrew installable on Mac OSX)


install
-------

- [Fork](https://github.com/sloria/dotfiles/fork) this repo.
- Clone your fork.

```bash
# Replace git url with your fork
# NOTE: It is important that you clone to ~/dotfiles
$ git clone https://github.com/YOU/dotfiles.git ~/dotfiles
$ cd ~/dotfiles
```

- Update the following variables in `group_vars/local` (at a minimum)
    - `git_user`: Your git user name.
    - `git_email`: Your git email address.
- Optional, but recommended: Update `group_vars/local` with the programs you want installed by [homebrew][], [homebrew-cask][], and npm.
    - `osx_homebrew_packages`:  Utilities that don't get installed by the roles.
    - `osx_cask_packages`: Mac Apps you want installed with [homebrew-cask][].
    - `npm_global_packages`: Node utilities.
- Edit `local_env.yml` as you see fit. Remove any roles you don't use. Edit roles that you do use.
- Run the installation script.

```bash
$ ./bin/dot-bootstrap
```

updating
--------

Once you have the dotfiles installed you can run the following command to rerun the ansible playbook:

```bash
$ dot
```

commands
--------

There are three main commands in the `bin` directory for setting up and updating development environments:

- `dot-bootstrap`: sets up local environment by executing all roles in `local_env.yml`.
- `dot`: updates local environment by executing all roles in `local_env.yml` except for the ones tagged with "bootstrap".
- `dot-remote`: sets up remote environments.

special files
-------------

All configuration is done in `~/dotfiles`. Each role may contain (in addition to the typical ansible directories and files) a number of special files

- **role/\*.zsh**: Any files ending in `.zsh` get loaded into your environment.
- **bin/**: Anything in `bin/` will get added to your `$PATH` and be made available everywhere.

notes
-----

**fonts**

To install fonts on OSX, simply drag the `.ttf` files in `misc/fonts` directory onto the Font Book application.

**python**

The `python` topic installs the [Anaconda Python distribution](https://store.continuum.io/cshop/anaconda/) using the [miniconda](http://conda.pydata.org/miniconda.html) installer. The installation is entirely self-contained, and lives at `~/miniconda`.

**iterm2 color themes**

256 color-mode should be enabled for themes to appear correctly. In the iterm2 preferences window, go to your default profile settings and click the "Terminal". Change the "Report Terminal Type" to "xterm-256color".

To import an iterm theme from the `misc/iterm-themes` directory, open your iTerm2 preferences and go to your Profiles settings. In the color tab, click the Load preset dropdown and choose "Import".


**macosx keyboard settings**

There are a few keyboard customizations that must be done manually:

- Turning repeat speed up to 11.

![Keyboard settings](https://dl.dropboxusercontent.com/u/1693233/github/dotfiles-mac-keys.png "Key repeat settings")


- Mapping Caps Lock to Ctrl.

![Modifier keys](https://dl.dropboxusercontent.com/u/1693233/github/dotfiles-mod-keys.png)

setting up remote dev environments
----------------------------------

The `remote_env.yml` playbook can set up a minimal subset of these dotfiles on remote machines.

- Copy `remotehosts.example` to `remotehosts`. The `remotehosts` file will note be added to version control.

```
$ cp remotehosts.example remotehosts
```

- Add the remote hosts you want to target under the `[remote]` group of the `remotehosts` file.

```
[remote]
123.456.789.111
```

Update the following variables in `group_vars/remote`:

- `dotfiles_user_home` : Your user home directory.
- `git_user` : Your git user name.
- `git_email` : Your git email.

Then run the `dot-remote` command.

```bash
$ ./bin/dot-remote
```

what if I only want your vim?
-----------------------------

The following commands will install vim-plug and download my `.vimrc`.

```
mkdir -p ~/.vim/autoload
curl -fLo ~/.vim/autoload/plug.vim https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
curl -fLo ~/.vimrc https://raw.githubusercontent.com/sloria/dotfiles/master/roles/vim/files/vimrc
```

You will now be able to open vim and run `:PlugInstall` to install all plugins.

todo
----

- Full Debian and Red Hat support
- Add more options to `dot` script, e.g. for skipping tasks

[homebrew]: http://brew.sh/
[homebrew-cask]: https://github.com/caskroom/homebrew-cask


license
-------

[MIT Licensed](http://sloria.mit-license.org/).
