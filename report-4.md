# Placeholder Title

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

#### Grand Total: 208 keypresses.
