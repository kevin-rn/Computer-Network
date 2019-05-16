### Debugging:
To better understand what is happening in the code, you can display the integers as a binary string for debugging purposes

Java:
Long.toBinaryString(6) => '110'

Python:
format(6, 'b') => '110'


### Select
To select parts of a binary sequence you can make use of bit masks. A bit mask is a binary sequence with 1s in the positions that are to be selected. This bit mask is then bitwise-AND'ed with the binary sequence.

For example:

10110110 (input) AND 11110000 (mask) => 10110000
AND -> &

### Moving
To move the selected part left or right in the sequence, bitwise-shifts can be used. A shift moves the entire sequence left or right depending on the chosen operator.

For example:

10110110 LEFT_SHIFT 1                 => 101101100
10110110 LEFT_SHIFT 2                 => 1011011000
10110110 RIGHT_SHIFT 1                => 01011011
10110110 RIGHT_SHIFT 4                => 00001011

Be careful when shifting numbers by more than 32 steps. This will overflow if the number is not a 64-bit integer 
RIGHT_SHIFT ->  >>
lEFT_SHIFT  ->  <<
for signed number use >>>

### Combining
To combine multiple bit sequences, a bitwise-OR can be used.

for example:

10110000 OR 00000110                     => 10110110
OR -> |

### Length
To calculate the length of the binary sequence (measured from the highest 1 bit), you can make use of:

Generic:

floor(log2(10110110)) + 1                   => 8

Java:

long bitSequence = Long.parseLong('10110110', 2)
Math.floor(Math.log(bitSequence)/ Math.log(2)) + 1      => 8


Python:

bit_sequence = int('10110110', 2)
bit_sequence.bit_length()                   => 8

### Bitcount
To calculate the amount of 1s in a binary sequence, you can make use of:

Generic:
bit_count(integer)
count = 0
while int
count += (int AND 1)
int = int RIGHT_SHIFT 1
return count



Java:
long bit_sequence = Long.parseLong('110', 2)
Long.bitCount(bit_sequence) => 2

Python:
bit_sequence = int('110', 2)
bit_count(6) => 2





