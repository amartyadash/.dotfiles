# .dotfiles
Uses a method described in [this](https://news.ycombinator.com/item?id=11070797) HackerNews thread to maintain dotfiles. Other references are

https://www.anand-iyer.com/blog/2018/a-simpler-way-to-manage-your-dotfiles.html

https://www.atlassian.com/git/tutorials/dotfiles
# First time setup
```
mkdir $HOME/.dotfiles
git init --bare $HOME/.dotfiles
```
We will make an alias for running git commands in our .dotfiles repository. I’m calling my alias dotfiles. Add this line to your .zshrc/.bashrc:

`alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME' `

From now on, any git operation you would like to do in the .dotfiles repository can be done by the dotfiles alias. The cool thing is that you can run dotfiles from anywhere.

Let’s add a remote, and also set status not to show untracked files:
```
dotfiles config --local status.showUntrackedFiles no
dotfiles remote add origin git@github.com:[yourusername]/.dotfiles.git
```

For an example:
```
cd $HOME
dotfiles add .tmux.conf
dotfiles commit -m "Add .tmux.conf"
dotfiles push origin master
```
To backup gnome terminal settings, run the following line:

`dconf dump /org/gnome/terminal/ > gnome_terminal_settings_backup.txt`
# Setting up a new machine
To set up a new machine to use your version controlled config files, all you need to do is to clone the repository on your new machine telling git that it is a bare repository:

`git clone --separate-git-dir=$HOME/.dotfiles https://github.com/[yourusername]/.dotfiles.git ~ `

However, some programs create default config files, so this might fail if git finds an existing config file in your $HOME. In that case, a simple solution is to clone to a temporary directory, and then delete it once you are done:

```
git clone --separate-git-dir=$HOME/.dotfiles https://github.com/[yourusername]/.dotfiles.git tmpdotfiles
rsync --recursive --verbose --exclude '.git' tmpdotfiles/ $HOME/
rm -r tmpdotfiles
```
To add gnome terminal settings run the following line:

`dconf load /org/gnome/terminal/ < gnome_terminal_settings_backup.txt`

# i3-gaps installation
```
sudo add-apt-repository ppa:kgilmer/speed-ricer
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install i3-gaps
```
Open the ~/.config/i3/config file and add these lines:

```
gaps inner 10
gaps outer 0
smart_gaps on
```
# i3blocks
```
git clone https://github.com/vivien/i3blocks
cd i3blocks
./autogen.sh
./configure
make
make install
```
Clone [i3blocks-contrib repo](https://github.com/vivien/i3blocks-contrib) in the home directory.

