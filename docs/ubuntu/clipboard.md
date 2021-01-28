
# Access Clipboard From Terminal In Ubuntu Using Xclip!


If You are going back and forth between terminal and any other application, accessing system clipboard contents from command line will be invaluable.


If You are using mac, there are inbuilt commands pbcopy & pbpaste. But in Ubuntu these are not available. You need to install a small utility called xclip. Go ahead and install it.


```
sudo apt-get install xclip
```


Now, You can copy any text ( or the output one command ) into the clipboard using xclip. To copy contents of fruits.txt to clipboard,

```
cat fruits.txt | xclip
```


If You want to see the contents of clipboard, You can use

```
xclip -o
```


This copy and paste will work only in the terminal, If You switch to another application and try to paste there, it wont work.

If You want to paste in another application, You need to copy like this

```
cat fruits.txt | xclip -selection clipboard
```


Now, You can switch to any other application & You can paste (CTRL + V) the contents.

Tip:
Instead of typing all of this everytime, You can setup alias in .bashrc file

```
alias c='xclip -selection clipboard'
alias v='xclip -o'
```

Now You can easily copy the contents like this

```
cat fruits.txt | c
```
