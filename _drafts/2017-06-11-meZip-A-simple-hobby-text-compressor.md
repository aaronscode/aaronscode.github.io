---
published: false
---
## Motivation

After recently finishing a class in Information Theory, I found myself motivated to do something to apply my newfound knowledge. There were a number of types of projects I could have persued (Information theory is a huge field and overlaps with many others), but after some deliberation, I settled upon implementing my own file compression utility. The end result is a simple python script with can compress and decompresess ASCII encoded plaintext files at a ratio that is comprable to gzip's lowest compression setting. Thinking myself clever, I've named my compression utility meZip, but it's actually a bit of a misnomer. Gzip uses the DEFLATE compression algorithm, which combines Huffman Coding and LZ77, while my utility is an implemetation of LZ78 (more on what that actually means later).

## Background

The basic premise behind compression is that you take a file shrink it by reducing number of bits used to encode the information in the file. This can be done in a lossless fashion, meaning that the file size is reduced but no actual information is lost, or in a lossy manner, meaning that some of the information in the file is sacrificied to make the compressed file even smaller. This article will only be concerned with lossles compression, but if you're interested in lossy compression, researching the JPEG image format is probably a good place to start.

but before I go any further, I should note that for every file that decreases in size when compressed, there is a file that will increase in size. This is a consequence of the fact that there are Information Theoretic hard limits on the ammount that you can compress a given block of information, and that every compression algorithm requires some inhernt overhead, whether that be in defining a symbol table or something else. For example, uncompressed plaintext English files compress very well because there is a significant ammount of redundancy in both the English language and the way we encode english characters. 

95 printable characters - 6.57 bits