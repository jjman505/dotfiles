# My dotfiles
This is my setup for a Unix/Linux environment. I can't guarantee it will work on your machine as well as it does on mine - this is just what works for me.

### Setup
Run `rake` inside the repo.

This will install zsh and oh-my-zsh, and symlink all of the config files from the repo into your home directory.

If the installer finds an existing file when symlinking, it will prompt you to: overwrite it, back it up (moved to a temp folder, which can be restored when 'uninstalling'), or skip it.

NOTE: You can run `rake brew` to install some common brew packages I use. This isn't recommended if you aren't me.

### Removal
Run `rake uninstall` inside the repo.

This will remove all of the symlinks and restore any backed up files.

### Multiple hosts
If you install these dotfiles on multiple hosts, you can put specific
local config settings in `~/.localrc` and they will be loaded.
