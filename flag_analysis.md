# Flags

## Add Folder
### File 1: add1.asm (8-bit addition)


    Overflow Flag, 1 : 130 surpasses the limit +127 limit,leaving the result on the  negative side.

    Sign Flag, 1 : The MSB of the result is 1

    Parity Flag, 1 : The result contains an even number of 1's

    Auxiliary Carry Flag, 1 : A carry occured between the  3rd and 4th bit

    Carry Flag, 0 :There is no carry from the 7th bit

    Zero Flag, 0 : Result of the operation is not 0

### File 2: add2.asm
    Adding 32000 + 500

    Overflow Flag, 0 : The result falls within range of the 16-bit signed range

    Carry Flag, 0 :There is no carry from the 7th bit

    Sign Flag, 0 :The result is positive, MSB is 0

    Parity Flag, 0 :The result contains 5 set bits (no. of 1s is odd)

    Auxiliary Carry Flag, 1 : A carry occured between the  3rd and 4th bit

    Zero Flag - 0 :The result of the operation is not 0


### File 3:  add3.asm (16-bit Addition with ADC)
[Demonstrating Carry Rollover and ADC]

##### 1. First Operation: `add ax, [num2]` (0xFFFF + 1)
    * **Result in AX:** `0x0000`

    Carry Flag, 1. Unsigned overflow: 65,535 + 1 = 65,536 (exceeds 16-bit limit of 65,535).

    Zero Flag, 1. Result in AX wrapped around to 0. 

    Auxiliary, 1. Carry occurred out of bit 3 during lower nibble addition

    Parity Flag, 1.The lower byte contains 0 set bits 

    Sign Flag, 0Bit 15 (MSB) is 0 

    Overflow, 0. Signed math (-1 + 1 = 0) produces a valid signed result


##### 2. Second Operation: `adc ax, 0` (0 + 0 + CF)
    * **Result in AX:** `0x0001`


    Carry Flag, 0. The sum 1 fits in 16 bits; carry is consumed and cleared

    Zero Flag, 0 .The result is now non-zero (`0x0001`)

    Parity Flag, 0. The lower byte (`0x01`) contains 1 set bit (odd parity)

    Sign,Overflow and Auxillary Carry,0 . No sign flip, signed overflow, or nibble carry occurred




## Division Folder

    For the div and idiv instructions,the status flags become undefined.This is because no meaningful geometric/logical mapping occurs, i.e., carry or overflow during an unsigned instruction.

    A divide error is raised separately if the quotient is too large or the divisor is 0



## Multiplication Folder

    For MUL and IMUL, they mainly affect Carry Flag and Overflow Flag because they indicate whether the result is too large to fit in the original operand size.

    The other flags  left undefined because they are not meaningfully determined by the multiplication operation.

### File 1: mul1.asm (8-bit multiplication)
    Multiply 25 * 10

    Carry Flag : 0 .The result 250 fits completely within the lower 8-bit registe(`AL`)so `AH` is 0.
    Overflow Flag : 0: Identical to Carry Flag 

### File 2: mul2.asm (16-bit multiplication)
    Multiply 3000 * 200

    Carry Flag 1 :The result of the operation (600000) exceeds 16-bit range, upper register is used so the Carry Flag is set to indicate that the upper register contains meaningful data

    Overflow Flag - 1 : Identical to Carry Flag

### File 3: mul3.asm (32-bit multiplication)
    Multiply 100000 * 300000

    Carry Flag, 1. The result of the operation (30000000000) exceeds 16-bit range, upper register is used so the Carry Flag is set to indicate that the upper register contains meaningful data

    Overflow Flag, 1. Identical to Carry Flag





## Subtraction Folder

### File 1: sub1.asm  
    Subtracting 50 - 80

    Carry Flag, 1. Acts the **Borrow Flag** 50 < 80, a higher bit was borrowed to complete the operation

    Sign Flag, 1 .The MSB of the result is 1, the result is negative.

    Overflow Flag, 0. The result of the operation (-30) is within the 8-bit signed range (-127 to +128)

    Zero Flag, 0. The result of the operation is not 0

    Parity Flag, 1. Number of 1s is even

    Auxiliary Carry Flag - 0. There was no borrow from the 4th bit to the 3rd bit.




### File 2: sub2.asm  16-bit subtraction
    Subtracting 1000 - 2000

    Overflow Flag, 0. The result of the operation (-1000) is within the 16-bit signed range (-32768 to +32767)

    Sign Flag, 1. The MSB of the result is 1, result is negative

    Auxiliary Carry Flag - 0. There was no borrow form the 4th bit to the 3rd bit

    Parity Flag - 1. Number of 1s is even

    Carry Flag - 1. 1000 < 2000, a higher bit was borrowed to complete the operation

    Zero Flag - 0. The result of the operation is  not 0

  
### File 3: sub3.asm / sbb.asm (16-bit Subtraction with SBB)
Subtracting 0 - 1, followed by SBB with 0

#### 1. After `sub ax, [num2]` (0 - 1 = 0xFFFF)
    Carry Flag, 1. Acts as the Borrow Flag; 0 < 1 so a higher bit was borrowed to complete the operation.

    Sign Flag, 1.The MSB of the result is 1, the result is negative (-1).

    Overflow Flag, 0. The result of the operation (-1) is within the 16-bit signed range (-32768 to +32767).

    Zero Flag, 0.The result of the operation is not 0 (`0xFFFF`).

    Parity Flag, 1.Number of 1s in the lower byte (`11111111b`) is even (8 set bits).

    Auxiliary Carry Flag, 1.A borrow occurred from the 4th bit to the 3rd bit ($0x0 - 0x1$).

#### 2.After `sbb ax, 0` (0xFFFF - 0 - CF = 0xFFFE)
    Carry Flag, 0.o further borrow was required; the borrow was consumed and cleared.

    Sign Flag, 1.** The MSB of the result is 1, the result is negative (-2).

    Overflow Flag, 0.** The result of the operation (-2) is within the 16-bit signed range.

    Zero Flag, 0.** The result of the operation is not 0 (`0xFFFE`).

    Parity Flag, 0.** Number of 1s in the lower byte (`11111110b`) is odd (7 set bits).

    Auxiliary Carry Flag, 0.** There was no borrow from the 4th bit to the 3rd bit.



