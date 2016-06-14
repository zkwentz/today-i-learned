```
~$: vim somefile.txt
CSApprox skipped; terminal only has 8 colors, not 88/256
Try checking :help csapprox-terminal for workarounds
```

Adding the following to my `.vimrc` file worked:

*.vimrc*
```
...
set t_Co=256
...
```
