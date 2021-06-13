# A tale of two endians  

If you've ever tried to make two computers speak with each other at any scale, be it just creating a 
socket connection or making an entire distributed network. You've probably heard the term endianness and the
troubles that it brings along. But before we delve deep into the topic lets make sure you actually have 
an idea how data is stored in a computer, make sure that you read it word by word :P.

## Number systems 

We are all familiar with indo-arabic number system, we've been taught numbers this way since pre-school that it is
literally second nature for us to think of numbers in this format. It takes less that second for us to read __123__ and to understand
that it means One hundred and twenty three. But we take for granted that there is a lot going on behind this system, we have assigned a place value and face 
value for each position and character and we can break down the process as follows

1. we assign each position a place value
    1. the left most position is assigned a place value of 1
    1. each position to the right is scaled by a factor of 10
1. we take the face value assigned to each character and multiply it by its place value 
1. add the results doing the above operation for each character

> 123 = 1 x 100 + 2 + 10 + 3 x 1
> each face value is multiplies by its place value and then summed up.

The entire process is quite well designed if you think about it, almost all our languages - english,hindi malayalam are all written and read from left to write, and the most significant bit that contributes most information to the number in consideration is on the left most end. This parallel to how languages are read is irrefutable. 

But giving it a bit more thought, that logic doesn't work does it. The number system is called indo-arabic. And in case you didn't know, arabic is written from right to left. Using that logic an arab would read the number with the least significant bit towards the right giving use the answer three hundred and twenty one.

With just a simple change in how we parse the number the entire meaning gets corrupted and thus we understand that data alone is not enough but the rules to parse it, is what gives data its meaning. In the above example, the explanation is quite simple, while the arabic language developed independently with its
right to left parsing style, the number system was adopted from the indians with whom many arabic traders interacted. The indian number system was a revolution at that time considered to the roman numeral system which is quite a mess. The arabs agreed that they would follow the rules followed by indians so that they can represent and communicate their data freely.

The bottom line is data by itself is meaningless, it depends on how the person interprets it, information lies in the mind of the beholder.

## Data in computers

Lets make a quick jump from out medieval friends to the 20'th century. Data is represented in computers 
as bits (1's and 0's) which signify high and low voltages in some sort of semi-conductor technology. The stream of bits by itself is quite meaningless we use a group of 8 bits to encode information, this unit
is called a __byte__. A byte can represent a range of 256 values and the encoding depends on implementation. 

In addition to this modern day computers don't load and store data byte by byte but as a unit of bytes, determined by the architecture it follows, bus width and register sizes. A typical 32 bit architecture has a word size of 4 bytes or 32 bits. Data is addressed by words and the computer can only operate on a word as a unit. Each word is assigned an address and the bytes inside while they have addresses cannot be used to load just that byte alone.

A word can be used to represent an integer and its values can vary from 0 to 2^32, but even then we come to the problem of how to parse a word

Consider an integer with hex value 0xDEADBEEF, it has a size of 4 bytes and can be placed in a single word in memory. We can make two choices here
in what order are the bits in a byte ordered, within the word how are the bytes ordered. Lets just take for granted that the bits in a byte are ordered in the same was as the indo-arabic numerals with most significant bit towards the left and so on. Then we have two choices on how to place the bytes within a word in memory, and they are shown in the figure below.

![endianness_example](./img/Endianness_example.png)

You would assume from our medieval tale that everyone agreed upon one rule for parsing and they shared 
data with each other happily ever after, but if that was the case you wouldn't be here reading this would
you. One of most glaring examples of immature behaviour by the tech industry is the issue of endianess. Both the above formats shown above exist simultaneously and any communication between two computers with varying architecture would require conversion between the two formats. 

The first example in the image above that follows the format that we write is called big endian, and the other as you might have already guessed the little endian format.

## Out in the wild 

Most of the really old architectures like IBM,PDP and SPARC followed the big endian architecture, then came a new player in the business who probably needs no introduction - Intel, intel's 8008 architecture followed little endian and all its descendants adopted it as well. As intel took over most 
of the consumer market almost all the processors out there follow the little endian format now.

When the byte-ordering for the standards of networking between machines was setup, the big endian architecture was chosen as network order, so for
transmitting any data that is over a byte long it has be corrected for to match the endianness, this is done using funcitons like __htonl()__ in C 
and specialized instructions like __bswap()__ in the processor level. Endianess has to be taken care when reading from filesystems of different byte-order.

## Find your byte order

Below is a simple program to find out what byte-order your system follows, it is an example from the extremely popular book Unix Network Programming by __Richard W Stevens__.

```c
#include <stdio.h>
#include <stdlib.h>

void main()
{

	/* 
		In a union the memory allocated will be of the maximum sized element, and all the
		members share the same memory.
	*/
	union
	{
		short s;
		char c[2];
	} a;
	a.s = 0x0102;	// Assign two bytes to a short
	if ( un.c[0] == 1 && un.c[1] == 2 )
		printf("Your byte-order is big endian");
	else if ( un.c[0] == 2 && un.c[1] == 1 )
		printf("Your byte-order is little endian");
}
```

## Conclusion

So now that we learned about byte-order and how to work around it, it all seems quite easy but to someone uninitiated
to this, the errors that this can cause can seem to defy logic. The workarounds are quite simple and it doesn't actually matter 
what order we use, but this story to me is lesson on why standards should be enforced and the need for collaboration between companies 
instead of making half baked solutions to a problem that shouldn't even have existed in the first place if they all just say in a room and came to an agreement.















