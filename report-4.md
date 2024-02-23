# Lab Report 4 - Vim

## Attempt 1: Without Shortcuts:
```console
ssh n2reed@ieng6-202.ucsd.edu<Enter>
```
- This opens an ssh connection to the ieng6 server (30 keypresses)
```console
git clone git@github.com:Minater247/lab7.git<Enter>
```
- This clones my lab7 clone's repository into ```./lab7``` (45 keypresses)
```console
cd lab7<Enter>
```
- This changes the current directory to ```./lab7```, where the repository is stored (8 keypresses)
```console
chmod +x ./test.sh && ./test.sh<Enter>
```
- This runs the test, and shows that it fails. (32 keypresses)
```console
vim ListExamples.java<Enter>
```
- This opens the ```vim``` text editor on the ```ListExamples``` file we want to edit. (22 keypresses)
```console
<Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Down><Right><Right><Right><Right><Right><Right><Right><Right><Right><Right><Right>
```
- This positions the cursor at the 1 we want to change (43 Down presses, 11 Right presses makes 54 keypresses)
```console
r2
```
- This replaces the character 1 with the character 2, which is the change we wanted to make. (2 keypresses)
```console
<Esc>:wq<Enter>
```
- This exits insert mode, saves the contents of `vim`'s buffer to the file, and quits `vim`. (5 keypresses).
```console
./test.sh<Enter>
```
- Finally, we run the tester again to see that all tests pass! (10 keypresses)

#### Grand Total: 208 keypresses, and way more time than necessary.

## Attempt 2: Some Basic Tricks
```console
<Up><Enter>
```
- This fetches the command `ssh n2reed@ieng6-202.ucsd.edu` from my bash history and runs it again, avoiding typing it again. (2 keypresses)
```console
gc <Right Click><Enter>
```
- If you're saying "gc? That's not a command!", you'd be right! I've used an alias to run `git clone` in place of whenever I use `gc`, using the command `alias gc='git clone'` in the `~/.bashrc` file. I then had the github repository copied to my clipboard, so all I had to do was right click to paste it in. I'm assuming for this that we have no previous commands on the ssh buffer to make use of. (5 keypresses)
```console
cd l<Tab><Enter>
```
- Pretty simple, `cd` into the newly created `lab7` directory from the repository, using tab completion for the name. In my case, no other filenames conflict, so only that much is necessary. (6 keypresses)
```console
bash ./t<Tab><Enter>
```
- This uses tab completion to run the test using bash, showing that it fails. (10 keypresses)
```console
vim *<Enter>
```
- At first glance, that doesn't seem right, does it? Actually, this is a little trick to cut down on keypresses - since alphanumerically, the file `ListExamples.java` comes first in the directory, we can use `*` to access everything - as long as we exit with `:wq!`, which saves and quits ignoring other buffered files, it will have us edit `ListExamples.java` first! (6 keypresses)
```console
?1 <Enter>
```
- This looks for the last instance of the string `'1 '` in the file, including the space. This turns out to be the 1 that we want to change to a 2, so that works well! (4 keypresses)
```console
r2
```
- The same as before, this replaces the current character, the 1, with a 2. (2 keypresses)
```console
:wq!<Enter>
```
- This saves the file, and exits - bypassing errors regarding other open buffers. (5 keypresses)
```console
bash t<Tab><Enter>
```
- Finally, we run the tester again to see that all tests pass! (10 keypresses)
#### Grand Total: 50 keypresses. Significantly better!

## Attempt 3: Pushing The Limits
```console
<Up><Enter>
```
- Same as before, this fetches the command `ssh n2reed@ieng6-202.ucsd.edu` from my bash history and runs it. (2 keypresses)
```console
gc <Right Click><Enter>
```
- Once more, I use the alias in the `~/.bashrc` file and the github repository copied to my clipboard, so all I had to do was run my `gc` alias and paste in the github repo. (5 keypresses)
```console
cd l<Tab><Enter>
```
- `cd` into the newly created `lab7` directory from the repository, using tab completion for the name. (6 keypresses)
```console
bash ./t<Tab><Enter>
```
- This uses tab completion to run the test using bash, showing that it fails. (10 keypresses)
```console
vim *.j*<Enter>
```
- Once again, I use the trick of opening every file to get the file we want, so `vim` is now open to the proper file. (9 keypresses)
```console
bash ./t<Tab><Enter>
```
- Wait, the tester again? We just opened `vim`, yet it's time for the tester to run. You see, I added something to the `~/.vimrc` this time, that automatically runs the find, replaces the character, saves, and exits all buffers! The `vimrc` contents are as follows:
```vim
augroup AutoCmds
  autocmd!
  autocmd VimEnter * execute "normal! ?1 \<CR>r2:wq!\<CR>"
augroup END
```
What that means is that the tester now passes, and we're done! However, this means any time we start up `vim` to do something else, we have to comment out those lines in the `vimrc`. (10 keypresses)
#### Grand Total: 42 keypresses. Pretty good!

## Attempt 4: Heavy Optimization
```console
<Up><Enter>
```
- Once again, this fetches the command `ssh n2reed@ieng6-202.ucsd.edu` from my bash history and runs it. (2 keypresses)
```console
gc <Right Click> .<Enter>
```
- I once again use the `~/.bashrc` alias, but this time I clone into whatever directory the `ssh` has dropped me into by passing `git` the current directory `.`. This is impractical as it could overwrite something else, but it does optimize out quite a few keypresses by avoiding `cd`. (7 keypresses)
```console
t<Enter>
```
- I have once again used the `~/.bashrc` to make an alias - this time mapping the alias `t='bash ./test.sh'`. This runs the tester, showing the errors! (2 keypresses)
```console
l<Enter>
```
- This is yet again another alias - this time mapping `l='vim ListExamples.java'`. This runs `vim` with the file ordering trick once again! (2 keypresses)
```console
t<Enter>
```
- Finally I run the tester again, showing that everything passes! (2 keypresses)
#### Grand Total: 15 keypresses!
I could optimize more, but I wanted this to at least be able to be run on any git repository saved to the clipboard. Additionally, by combining the aliases using &&, it is possible to save 4 more keypresses, but since we haven't learned that yet, I've left it out.
