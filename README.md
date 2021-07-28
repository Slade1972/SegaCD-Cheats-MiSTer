# SegaCD Cheats for MiSTer
Cheat files for the Sega CD or Mega CD in .gg format compatible with MiSTer

Gamehacking.org unfortunately does not have a large number of Sega CD or Mega CD cheats, and those it does have are not produced in a correct or compatible format for the MiSTer.
The following repository is a list of cheats created for use within MiSTer.

Cheat files need to be placed in the same directory as the game files (.cue and .bin) and should have the same name.
All cheats are tested thoroughly on an emulator as well as on MiSTer itself.

Credit for cheats found by other people will be placed in the readme in each file.

## Cheat Format
The cheat file format is slightly different for the Sega CD or Mega CD. The file is made up of four 32bit addresses, as usual, but has a different set of flags.

`00 00 00 00` The first four bytes set the compare flag. This can be `00` for no compare and word write (`00 00`), `01` for a compare with word write (`00 00`), `02` for a byte write (`00`) with no compare and `03` with a byte write (`00`) with compare.

`00 00 00 00` The second four bytes are the ram location in little endian (this is byte swapped or reverse order from the Sega CD or Mega CD). If the ram location is in CD PRG Ram, you will need to append `FF` as the last byte. 68k ram requires that `FF` is appended as the third byte, with `00` as the last byte. The 68K ram is 64kBytes in length, meaning that the lowest ram location is `00 00` and the highest is `FF FF`. So all 68k ram locations will take up no more than two bytes of the address. The remainder should be filled with `FF 00`.

`00 00 00 00` The third four bytes are the compare value. This is only required if you set the compare bit and should be a byte `00` or word `00 00` value. This is designed to allow cheats to see if there is a particular value set before overriding it with something else.

`00 00 00 00` The final four bytes are the write value. This is either a byte or word in length, in little endian. If you've set at `02` or `03` as the compare flag, then you'll write a byte value, otherwise word value. Trailing bytes should be `00 00` to `00 00 00` to make up the double word length.

Using this, a cheat for Player 1 Infinite Health in 3 Ninjas Kick Back looks like this:

`02 00 00 00` `4B B0 07 FF` `00 00 00 00` `06 00 00 00`

The compare flag is `02` so it's a byte write. The location in big endian is `07 B0 4B` and the `FF` indicates this is CD PRG Ram. There is no compare value, and the value to write is `06`. This means you will always have 6 health throughout the game (or the levels that this works in).

## Multi Line Cheats
So, what happens if the game writes to multiple locations?

Simple, you write multiple lines. The cheat file is just a binary file, and it can contain multiple lines, provided they follow the correct sequence, the cheat engine in MiSTer will happily read each line and apply the correct cheat. In some games, such as Earthworm Jim, this is required due to the way the game stores values.
