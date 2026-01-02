## Neovim commands 

- dw delete the word until the start of the next word
- de delete to the end of the current word
- d$ delete to the end of the line
- x delete below the cursor
- A append to the text
- <n>w move cursor n words forward
- <n>e move cursor to the end of the n word forward
- d<n>e , d<n>w delete n words forward / end of the n words forward
- dd deletes the line and store it (like cut)
- p pastes the line (after cursor) stored via dd 
- P pastes line before cursor
- r replace, press r then type 
- ce change until the end of the word, (c difference with d is that it deletes the word and goes to insert mode)
- c$ change until the end of the line 
- G move to the end of the file
- gg move to the start of the file
- C-g tells you the position you are on the file 
- line-number+G takes to the line
- / follwing w a phrase search forward
- ? search backwards
- n next search
- N next search opposite direction
- % on a ( [ { to match 
- :s/old/new subtitute old for new, first osne
- :s/old/new/g all olds in one line
- :%s/old/new/g all the olds in the file, adding c to ask for confirmation :%s/old/new/gc
- :#,#s/old/new between two lines 


