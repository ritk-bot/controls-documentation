## Problems with not showing up on lsusb for STMF411CEU6 black pill

- unplug usb c 
- while holding boot0, replug and keep holding for a few seconds before letting off 
- check lsusb again

## Problems with STMF411CEU6 black pill board not writing 

- go to option bytes on cube programmer
- in that menu go to "write protection"
- see if all are checked (checked means write protection is off)

## How to erase flash memory
- Go to erase and programming in the left pane
- Then go to Erase flash memory tab and click on full chip erase
- you can check if it actually worked by going to the home (Memory and file editing pane), clicking and read and then seeing if the bytes show 'FFFFF' 
