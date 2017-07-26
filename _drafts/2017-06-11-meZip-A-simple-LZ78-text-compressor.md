---
published: false
---
## Introduction

In this post I'll be talking about a toy file compression utility I wrote in Python. I'll start off with a brief overview of the subject of compression for the uninitiated, and then transition into a step-by-step example,

After recently finishing a class in Information Theory, I found myself motivated to do something to apply my newfound knowledge. There were a number of types of projects I could have persued (Information theory is a huge field and overlaps with many others), but after some deliberation, I settled upon implementing my own file compression utility. The end result is a simple python script with can compress and decompresess ASCII encoded plaintext files at a ratio that is comprable to gzip's lowest compression setting. Thinking myself clever, I've named my compression utility meZip, but it's actually a bit of a misnomer. Gzip uses the DEFLATE compression algorithm, which combines Huffman Coding and LZ77, while my utility is an implemetation of LZ78 (more on what that actually means later).

## Background

The basic premise behind compression is that you take a file shrink it by reducing number of bits used to encode the information in the file. This can be done in a lossless fashion, meaning that the file size is reduced but no actual information is lost, or in a lossy manner, meaning that some of the information in the file is sacrificied to make the compressed file even smaller. This article will only be concerned with lossles compression, but if you're interested in lossy compression, researching the JPEG image format is probably a good place to start.

but before I go any further, I should note that for every file that decreases in size when compressed, there is a file that will increase in size. This is a consequence of the fact that there are Information Theoretic hard limits on the ammount that you can compress a given block of information, and that every compression algorithm requires some inhernt overhead, whether that be in defining a symbol table or something else. For example, uncompressed plaintext English files compress very well because there is a significant ammount of redundancy in both the English language and the way we encode english characters. 

95 printable characters - 6.57 bits

## An Example

Let's consider the sequence of letters:

	ABABABABABABABABABABABABABABABAB

What are the ways we could represent this in binary? Well, we could use the ASCII character encoding, in which case the above sequence would look something like this:

	01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010 01000001 01000010

Unfortunately, each character is taking up eight whole bits, which is an enormous waste of space! We'd be much better off if we just assigned a 0 to 'A' and a 1 to 'B,' yielding:

	010101010101010101010101010101

And just like that we reudced the number of bits we were using by 8x. This is actually the result we would get if we applied the [Huffman Coding algorithm](https://en.wikipedia.org/wiki/Huffman_coding) to our example sequence. But we can do even better! Really, our example is just the phrase 'AB' repeated fifteen times, and this is one way we could encode that:

	011111

The first bit is 0, for 'A,' the second is 1 for 'B,' and the next four bits encode the number '15' for "repeat the previous sequence 15 times". Obviously, this encoding is highly dependent on the exact example we're looking at, and to recover the original message you need to undersand how to decode the binary sequence above. It is, however, general enough to encode the sequence:

	BABABABABABABABABABABABABABABABA

as

	101111
    
using the same rules as before.
    
What if, instead of encoding the whole sequence of A's and B's, our only choice was between whether the sequece started with an A or a B:

	ABABABABABABABABABABABABABABABAB
	BABABABABABABABABABABABABABABABA

In this case, there is only one "bit" of information we need to convey, whether the sequence starts with 'A' or 'B.' This means we could represent the entire sequence starting with an A as a 0, and our sequence starting as a B with 1. As you can see, there are all sorts of little games you can play to squeeze as much compression out of your data as possible if you know enough about the sequnce you're trying to encode, or can assume some constraints on it. For example, when we were encoding our sequence as "first two letters, then the number of times we repeat those" above, we gave ourselves 4 bits to described the length, so we implicitly assumed it would be between 0 and 15. To encode a sequnce longer than 15 repeated charcters, we'd need to at least one extra bit.

This begs the question, what if the data we want to compress isn't some hand picked sequence of charcters chosen for the sake of walking through some examples? What if we don't know how long the data will be, what the distribution of symbols (symbols are letters, in our case) is, or even how many symbols there are? There could be more than just A or B in some text we want to compress; we have to consider other letters, and also things like newlines.

Huffman Coding is able to address some of the issues above, but I wanted to implement a [Universal Source Coding](http://www.eit.lth.se/fileadmin/eit/courses/eit080/InfoTheorySH/InfoTheoryPart2a.pdf) algorithm, which is able to address all of the issues raised above, and more. Universal Soruce coding algorithms are also more interesting to implement than simple Huffman Coding, and I was looking for something that would be a challenge.

## The Lempel-Ziv '78 Algorithm

I point you towards [this resource](http://math.mit.edu/~goemans/18310S15/lempel-ziv-notes.pdf), as I found it useful when I was writing my implementation of LZ-78. Note that it leaves out one or two details regarding the more practical aspects of the implementation, but I go over these in my explanation.

## The Implementation

### Compression

### Decompression

## Afterthoughs
