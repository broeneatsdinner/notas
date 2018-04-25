# Editing files remotely via SSH on SublimeText 3

Sometimes you need to edit a file on a remote server, but using vim/emacs/nano is not very practical, due to lag and speed of screen refresh.

Maybe you want to use your local copy of [Sublime Text](http://www.sublimetext.com) for its syntax highlighting and familiar keyboard shortcuts.

There exists a way for you to create/open remote files on your local machine running Sublime Text, and edit them. When you save your changes in Sublime Text, the remote file is updated automagically.

This guide instructs you on how to do just that. The previous sentence assumes you read the above paragraphs to infer what "that" is.

***

Step by step:

1. _(on your local workstation)_ On Sublime Text 3, open Package Manager (Ctrl-Shift-P on Linux/Win, Cmd-Shift-P on Mac, _Install Package_), and search for `rsub`

2. _(on your local workstation)_ Add `RemoteForward 52698 127.0.0.1:52698` to your .ssh/config file, or `-R 52698:localhost:52698` to the end of your SSH command if you prefer command line

* Note: if you're using ~/.ssh/config and getting the error `/usr/local/bin/rsub: socket: Address family not supported by protocol`, reference this: https://stackoverflow.com/questions/18938950/rsub-with-sublime-and-ssh-connection-refusual

Currently, my ~/.ssh/config looks like this:
```
Host *
  RemoteForward 52698 localhost:52698
```

3. On your remote server:
```
sudo wget -O /usr/local/bin/rsub https://raw.github.com/aurora/rmate/master/rmate
sudo chmod a+x /usr/local/bin/rsub
```

***

Just keep your ST3 editor open on your local machine, and you can easily edit remote files by typing `rsub myfile.txt` in your terminal window.

BONUS: Create an alias and you can edit remote files with `subl myfile.txt`. To do so, edit ~/.bash_profile or ~/.bashrc and add the following:
```
# Dude, if you want to use Sublime Text to edit files on this machine,
alias subl='rsub'
```

EDIT: if you get "no such file or directory", it's because your `/usr/local/bin` is not in your PATH. Just add the directory to your path:
```
echo "export PATH=\"$PATH:/usr/local/bin\"" >> /etc/profile
```

Now just log off, log back in, and you'll be all set.
