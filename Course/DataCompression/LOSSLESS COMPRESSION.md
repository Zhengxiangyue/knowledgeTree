# **LOSSLESS COMPRESSION**

**Abdou Youssef**

1. [HUFFMAN CODING](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#huffman)

2. [RUN-LENGTH ENCODING](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#rle)

3. [GOLOMB CODING](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#golomb)

4. [ARITHMETIC CODING](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#arithmetic)

5. [LEMPEL-ZIV COMPRESSION](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#LZ)

6. [DIFFERENTIAL PULSE-CODE MODULATION](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#dpcm)

7. [BITPLANE CODING](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#bitplane)

8. [LIMITATIONS OF LOSSLESS COMPRESSION](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#limitations)

9. LOSSLESS COMPRESSION IN STANDARDS, GRAPHIC FILE FORMATS, AND UTILITIES

   ​

# [1. HUFFMAN CODING]()

-  Each (macro)symbol A has a probability PA 

-  Form a Huffman tree as follows:

   1. Create a node for each symbol
   2. While (there are two or more uncombined nodes) do
      -  select 2 uncombined nodes a and b of minimum probabilities
      -  Combine a and b by creating a new node c of prob Pa+Pb, and making a and b children of c
   3. Label the tree edges: left edges with 0, right edges with 1
   4. The code of each symbol is the binary sequence labeling the path from the root down to the corresponding leaf

   ​

-  Example: Alphabet={A,B,C,D,E,F,G,H} of probabilities 1/2, 1/4, 1/16, 1/16, 1/32, 1/32, 1/32, 1/32

   ​

-  Coding a Sequence/File

   -  Represent each symbol in the sequence by its Huffman code
   -  Example: ABBAACA is coded as 101011100101

   ​

-  Decoding

   -  Proceed from the next undecoded bit, and walk down the tree (starting from the root) going left or right depending on whether the bit is 0 or 1

   -  When a leaf is reached, replace the binary string just scanned by the symbol corresponding to the leaf 

   -  Example: (decode the example above)

      ```
      	
      ```

Back to Top

\2. RUN-LENGTH ENCODING

-  Represent each subsequence of identical symbols by a pair (L,a) where L is the length of the subsequence, and a is the recurring symbol in the subsequence 
-  Example: aaabbbbaaaa is coded as (3,a) (4,b) (4,a) 
-  If the sequence is binary, there is no need to represent a because the value of a alternates between 0 and 1 
-  Example: 00011111000011 is coded as 3,5,4,2 
-  RLE can be followed by Hoffman coding to further code the Ls and a's 
-  The fax standard uses RLE

Back to Top

\3. GOLOMB CODING

-  Golomb coding is a practical and powerful implementation of Run-Length Encoding of binary streams.

   ​

-  The Method: Golomb Coding of order m (where m is a positive integer set by the algorithm implementer or by the user; its optimal value will be shown later to be the nearest power of 2 to p*Ln(2)/(1-p), where p is the probability of the more probable bit in the input stream)

   1. Assume 0 is the more probable bit
   2. Break the input into runs of the form 0i1. Call the last bit of each run the **tail **bit. 
      Remark: the last run may or may not have a tail. For example, in 0010001, the last run (0001) has a tail (1), while in 001000, the last run (000) has no tail.
      Remark: if the more probable bit is 1, then each run is of the form 1i0.
   3. Code each 0i1 as 1q 0 y, where 
      i=qm+r (0 <= r < m), that is, do an integer division of i by m, take q as the quotient and r as the remainder;
      y is the binary representation of r, using log2 m bits.
   4. the final coded bitstream is: **MPb code1 code2 ... coden tail? **where 
      **MPb**: is the (1-bit) value of the more probable bit 
      **codej**: the code of the j-th run 
      **tail?**: a single bit that is 1 if the last run has a tail; it is 0 if the last run has no tail.

   ​

-  Example: take as input the stream x=0

   10

   10

   14

   11

    

   ​

   -  0 is the more probable bit, since it occurs 24 times, while 1 occurs 3 times.
   -  p=24/27.
   -  p*Ln(2)/(1-p) = 5.6 (note that Ln(2) = 0.6931)
   -  The closest power of 2 to 5.6 is 4. Thus, m=4 and log m = 2
   -  MPb = 0
   -  The runs of x are 0101, 0141, and 001
   -  10=2*4 + 2, that is, q=2 and r=2=10 in binary using 2 bits. Thus, the 1st run codes to 12 0 10
   -  14=3*4 + 2, that is, q=3 and r=2=10 in binary. Thus, the 2nd run codes to 13 0 10
   -  0=0*4 + 0, that is, q=0 and r=0=00 in binary using 2 bits. Thus, the 3rd run codes to 0 00.
   -  tail? = 1 because the last run has a tail.
   -  Therefore, the code for x is 0 11010 111010 000 1.
   -  CR=27/16=1.69.

   ​

-  Theorem: The optimal value of m is the nearest power of 2 of p*Ln(2)/(1-p), where p is the probability of 0.

   ​

-  Proof of the Theorem.

   -  Assume 0 is the more probable bit.
   -  Let L be the number of 0s in any arbitrary run 0L1
   -  L is a **random variable** that takes on values in {0,1,2,...}
   -  Prob[L=n]=Prob[1st bit=0]Prob[2nd bit=0]...Prob[nth bit=0]Prob[(n+1)st bit=1]
   -  Prob[L=n]=pn(1-p)
   -  The average (or expected value) of L isE(L)=0*Prob[L=0]+1*Prob[L=1]+2*Prob[L=2]+ ... +n*Prob[L=n] + ...E(L)=(1-p)[0*p0 + 1*p1 + 2*p2 + 3*p3 + ... + n*pn + ... ]E(L)=p(1-p)[1*p1-1 + 2*p2-1 + 3*p3-1 + ... + n*pn-1 + ... ] + ...]E(L)=p(1-p)[1/(1-p)2]
   -  E(L)=p/(1-p)
   -  Now, the task is to find m such that the length of the code for 0E(L)1 is minimized
   -  Let E(L)=qm+r, where q=floor(E(L)/m), and 0<= r <= m-1
   -  The code of 0E(L)1 is 1E(L)/m0(log m bits for r). Note that we removed the floor notation for simplification of analysis, without much loss of prescision.
   -  The length of that code is: E(L)/m+1+log m = p/((1-p)m) + 1 + Ln(m)/Ln(2)
   -  Call that length f(m):f(m)=p/((1-p)m) + 1 + Ln(m)/Ln(2)
   -  To find the value of m that minimizes f(m), compute the derivative f'(m) and set it to 0. Note that we're turning m into a real (as opposed to integer) variable so we can apply calculus, especially differentiation for optimization.
   -  f'(m)=(1/m) * 1/Ln(2) - p/((1-p)m2)
   -  f'(m)=0 implies that m = Ln(2) * p/(1-p)
   -  It can be easily verified that for m < Ln(2) * p/(1-p), f'(m) < 0, and for m > Ln(2) * p/(1-p), f'(m) > 0. Thus, at m=Ln(2) * p/(1-p), f(m) achieves a minimum (rather than a maximum).

## Differential Golomb

-  If 0 is only slightly more probable than 1, then Golomb coding does not compress the data at all. Indeed, when 0 and 1 are nearly probably, this is what happens:
   -  the optimal value of m will be equal to 1
   -  there would be long runs of 1s, (we call them "1-runs")
   -  each 1-run translates to as many Golomb runs, of the form 001, as there are 1s in the 1-run,
   -  each Golomb run 001 codes to 100=0
   -  therefore, each 1-run 1n codes to 0n. That is, no compression is achieved.
-  Differential Golomb improves the situation considerably.
-  Method:
   1. Let the input stream be x=x1x2 ... xn
   2. Transform x to z=z1z2 ... zn where zi=xi-xi-1 for i>1 and z1=x1
   3. Observe that z is a sequence of runs of the form 0i1 or 0i(-1). Observe also that the 1 and -1 in those runs **alternate **, and the first run has a 1 (rather than a -1).
   4. Because of the alternation of the sign of 1, we can drop the sign, and the decoder can always remember where the negative signs are. Call the resulting stream z'.
   5. Code z' using Golomb coding.

Back to Top

\4. ARITHMETIC CODING

-  Arithmetic Coding achieves a bit rate equal to the entropy 

-  It codes the whole input sequence, rather than individual symbols, into one codeword 

-  The Conceptual Main Idea

   -  For each binary input sequence of n bits, divide the unit interval into 2n intervals, where the length of i-th interval Ii is the probability of the i-th n-bit binary sequence 
   -  Code the i-th binary sequence by r1r2...rt where 0.r1r2...rt... is the binary representation of the right end of interval Ii, and t=![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/arithmceil.gif)

   ​

-  Method

   1. Let a1a2...an be the input to be coded
   2. Let I=[L,R) be the interval corresponding to the subsequence scanned so far
   3. Initially, I=[0,1);
   4. for i=1 to n do
      -  Let Pi=Prob[0/a1a2...ai-1]
      -  ![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/Delta.gif)=R-L
      -  Divide I into 2 intervals: [L,L+Pi![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/Delta.gif)) and [L+Pi![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/Delta.gif), R)
      -  If ai=0, reduce I to [L,L+Pi![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/Delta.gif))
      -  Else, reduce I to [L=L+Pi![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/Delta.gif),R)
   5. t=![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/arithmceil2.gif)
   6. Express R in binary R=0.r1r2...
   7. Code the input with r1r2...rt and add to the code the length n of the input stream.

   ​

-  Patent: IBM Q-Coder 

-  Example: Binary Markov Source P[0/0]=P[1/1]=3/4, P[0/1]=P[1/0]=1/4 and P[0]=P[1]=1/2 (Draw the example)

   $$

-  This algorithm quickly runs into precision (underflow) problems.

-  Different implementation methods for dealing with this imprecision problem have been devised. They are not covered here.

-  Also, interesting ways for deriving the probabilities have been developed but will not be presented here.

Back to Top

\5. LEMPEL-ZIV COMPRESSION

-  LZ encodes recurring patterns (blocks) using the positions of their first occurrences 

-  LZ Encoder

   1. Let x1x2...xn be the input to be coded
   2. Maintain a dictionary (DICT) of patterns seen so far
   3. DICT[1]=x1, and put x1 in the output code
   4. i=2;
   5. While (there are still input symbols) do
      -  Read from the remaining input until the string scanned is no longer in DICT. Call that string Wa, where W is in DICT and a is the input symbol after W
      -  Let j be the index where W=DICT[j];	(j < i)
      -  Let J be the ![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/logiceil.gif)-bit binary representation of j.
      -  Code Wa as (J,a) and append that code to the output
      -  DICT[i]=Wa
      -  i=i+1

   ​

-  Remark: The dictionary is not stored/transmitted. 

-  The LZ bitrate is asymptotically optimal **without** the need to know or compute the underlying probability model of the input data.

-  Example: x=0010100100010010100110101

    

   ​

   ​

   | i    | log i | J=j      | W     | a    | DICT[i] |
   | ---- | ----- | -------- | ----- | ---- | ------- |
   | 1    | 0     | empty    | empty | 0    | 0       |
   | 2    | 1     | (1)2=1   | 0     | 1    | 01      |
   | 3    | 2     | (10)2=2  | 01    | 0    | 010     |
   | 4    | 2     | (11)2=3  | 010   | 0    | 0100    |
   | 5    | 3     | (100)2=4 | 0100  | 1    | 01001   |
   | 6    | 3     | (101)2=5 | 01001 | 1    | 010011  |
   | 7    | 3     | (011)2=3 | 010   | 1    | 0101    |

   ​

   ​

   Code of x: 0 (1,1) (10,0) (11,0) (100,1) (101,1) (011,1)

    

   ​

   Code of x in a compact form (the true form): 011100110100110110111

   ```

   ```

-  LZ Decoder

   1. Let y=y1y2... be the codeword to be decoded back to x
   2. x=y1 and DICT[1]=y1
   3. i=2
   4. While (the codeword is not fully scanned) do
      -  j= the next ![img](http://www2.seas.gwu.edu/~ayoussef/cs6351/equations/logiceil.gif) bits from y
      -  W=DICT[j]
      -  a= the next symbol from y
      -  append Wa to the right of x
      -  DICT[i]=Wa
      -  i=i+1

   ​

-  Example: Decoding y=011100110100110110111 from the previous example

| i    | log i | J=j      | W=DICT[i-1] | a    | DICT[i] | x= (previous(x))(Wa)      |
| ---- | ----- | -------- | ----------- | ---- | ------- | ------------------------- |
| 1    | 0     | empty    | empty       | 0    | 0       | 0                         |
| 2    | 1     | (1)2=1   | 0           | 1    | 01      | 001                       |
| 3    | 2     | (10)2=2  | 01          | 0    | 010     | 001010                    |
| 4    | 2     | (11)2=3  | 010         | 0    | 0100    | 0010100100                |
| 5    | 3     | (100)2=4 | 0100        | 1    | 01001   | 001010010001001           |
| 6    | 3     | (101)2=5 | 01001       | 1    | 010011  | 001010010001001010011     |
| 7    | 3     | (011)2=3 | 010         | 1    | 0101    | 0010100100010010100110101 |

Back to Top

\5. DIFFERENTIAL PULSE-CODE MODULATION (DPCM)

-  DPCM is a predictive technique that capitalizes on inter-pixel spatial redundancy 
-  DPCM predicts the next pixel based on the values of the previous neighboring pixels 
-  It then computes the residual pixel (actual - predicted) 
-  Finally, it losslessly compresses the residual data, using RLE, Huffman, etc. 
-  DPCM decorrelates the data and causes the residual to have lower (memoryless) entropy 
-  DPCM is the lossless JPEG standard 
-  Prediction methods:
   -  For 1D signals: Let x1, x2, ... be a 1D signal. The predicted value of xk isyk = a*xk-1where a is a parameter.
   -  Good values for a are 1, 0.99.
   -  For 2D signals: Let A be an image matrix. The predicted value of A(i,j) isB(i,j)=aA(i,j-1)+bA(i-1,j-1)+cA(i-1,j)where (a,b,c) are parameters of 2D DPCM
   -  Good values for (a,b,c) are: (1,0,0), (0,0,1), (.5,0,.5), (1,-1,1), and (.75,-.5,.75).

Back to Top

\7. BITPLANE CODING

-  Let I be an image where every pixel value is n-bit long
-  Express every pixel in binary using n bits
-  Form out of I n binary matrices (called bitplanes), where the i-th matrix consists of the i-th bits of the pixels of I.
-  Example: Let I be the following 2x2 image where the pixels are 3 bits long101110111011The corresponding 3 bitplanes are:1     01     10     11     11     11     0
-  Bitplane coding: code the bitplanes separately, using RLE (flatten each plane row-wise into a 1D array), Golomb coding, or any other lossless compression technique.

## Gray-Code Implementation

-  In natural images, nearby pixels are identical or (mostly) near-identical.

-  Problem: pixel values that differ by 1 or 2 in decimal differ in many bit positions in binary. For example, take two neighboring pixels of values 7 and 8, which are rather similar. Express those value in 4-bit binary: 0111 and 1000. The binary strings differ in every bit position. In other terms, in each bit plane, the pixels in the same relative positions differ, thus reducing the potential for compression.

-  Goal: A different binary representation where similar decimal values differ in only a small number of bits.

-  Solution: Gray Coding

-  Definition: a Gray code of n bits a sequence of up to 2n binary strings of n bits each, such that any two successive strings differ by only one bit.

-  Construction method for Gray codes:

   1. Let Gn denote the Gray code of n bits.
   2. G1 = 0, 1
   3. Gn = 0Gn-1, 1GRn-1 where GRn-1 is Gn-1 listed backward (i.e., last string of Gn-1 is the first string of GRn-1).
   4. Examples: 
      G2 = 00, 01, 11, 10 
      G3 = 000, 001, 011, 010, 110, 111, 101, 100

-  The Gray code of a number m is the m-th string in Gn. For example, the 3-bit Gray code of 2 is 011, of 5 is 111, and of 7 is 100.

-  A complete example:

   -  Let I be the following 3-bit image:556554546544445455334344234323123212

   -  The left column below corresponds to straight binary bitplane coding, and the right column corresponds to Gray-code bitplane coding

      | I101101110101101100101100110101100100100100101100101101011011100011100100010011100011010011001010011010001010 | IG111111101111111110111110101111110110110110111110111111010010110010110110011010110010011010001011010011001011 |
      | ---------------------------------------- | ---------------------------------------- |
      |                                          |                                          |
      | I0: 24 runs110110100100001011110100010101101010 | IG0: 17 runs111110101100001011000000100010110111 |
      |                                          |                                          |
      | I1:16 runs001000001000000000110100110111011101 | IG1:9 runs110111110111111111111111111111011101 |
      |                                          |                                          |
      | I2:8 runs111111111111111111001011001000000000 | IG2:8 runs111111111111111111001011001000000000 |
      | Total number of runs: 48                 | Total number of runs: 34                 |

   -  The actual runs in the G bitplanes are: 
      IG0: 5, 1, 1, 1, 2, 4, 1, 1, 2, 6, 1, 3, 1, 1, 2, 1, 3
      IG1: 2, 1, 5, 1, 21, 1, 3, 1, 1 
      IG2: 18, 2, 1, 2, 2, 1, 9

   -  The "alphabet" of those run lengths is: {1,2,3,4,5,6,9,18,21}

   -  The corresponding frequencies are: 17, 7, 3, 1, 2, 1, 1, 1, 1

   -  The huffman codes of those run lengths are:

      ​

      | Symbols | Huffman Code |
      | ------- | ------------ |
      | 1       | 1            |
      | 2       | 01           |
      | 3       | 011          |
      | 4       | 0100         |
      | 5       | 0101         |
      | 6       | 00010        |
      | 9       | 00000        |
      | 18      | 00001        |
      | 21      | 00011        |

      ​

   -  the total number of bits of coding IG0,IG1,IG2 is: 17*1+7*2+4*3+5*1+5*2+5*1+5*1+5*1+5*1=78 bits.

   -  The bitrate is: 78/36 = 2.17 bits per pixel 

   -  The actual runs in the I planes are: 
      I0: 2, 1, 1, 1, 2, 1, 4, 1, 1, 4, 1, 1, 3, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1 
      I1: 2, 1, 5, 1, 9, 2, 1, 1, 2, 2, 1, 3, 1, 3, 1, 1, 
      I2: 18, 2, 1, 2, 2, 1, 9

   -  The "alphabet" of those run lengths is: {1,2,3,4,5,9,18}

   -  The corresponding frequencies are: 28, 11, 3, 2, 1, 2, 1

   -  The Huffman codes of those run lengths are:

      ​

      | Symbols | Huffman Code |
      | ------- | ------------ |
      | 1       | 1            |
      | 2       | 01           |
      | 3       | 0011         |
      | 4       | 0010         |
      | 5       | 00000        |
      | 9       | 0011         |
      | 18      | 00001        |

      ​

   -  The total number of bits for I0,I1,I2 is: 28*1 + 11*2 + 4*3 + 4*2 + 5*1 + 4*2 + 5*1 = 88 bits > 78 bits for Gray codes

   -  Bitrate: 88/36=2.44 > 2.17

   ​

   ​

   ​

   ​

-  Method of conversion between regular binary representation and Gray code:

    

   ​

   -  Let x be decimal number whose binary representation isbn-1...b1b0and whose Gray code isgn-1...g1g0
   -  Binary ---> Gray: gn-1=bn-1, and gi=bi **XOR** bi+1 for i=0,1,2,...,n-2
   -  Gray ---> Binary: bn-1=gn-1, and bi=gi **XOR** gi+1 **XOR** ... **XOR** gn-1 for i=0,1,...,n-2

Back to Top

\8. LIMITATIONS OF LOSSLESS COMPRESSION

-  Low compression ratios (about 2 to 1, and in some cases up to 5 to 1)
-  No lossless compression technique can compress every possible input by at least one bit

Back to Top

\9. LOSSLESS COMPRESSION IN STANDARDS, GRAPHIC FILE FORMATS, AND UTILITIES

| Standards                                | Compression                  |
| ---------------------------------------- | ---------------------------- |
| [JBIG](http://www.jbig.org/jbighomepage.html) and [JBIG 2](http://www.jbig.org/fcd14492.pdf) | Arithmetic coding            |
| Grayscale and color JBIG                 | bitplane + Arithmetic coding |
| Lossless [JPEG](http://www.jpeg.org/)    | DPCM                         |
| Fax: - Group 3 - Extended 2D Group 3 - Group 4 | RLE and Huffman coding       |

| Graphic Format                           | Compression                              |
| ---------------------------------------- | ---------------------------------------- |
| BMP (Microsoft)                          | RLE                                      |
| GIF (CompuServe)                         | LZW                                      |
| TIFF                                     | Choice of Group 3, Group 4, LZW, or RLE  |
| [PNG](http://www.libpng.org/pub/png/spec/png-1.2-pdg.html) | a variant of LZ77 (optionally preceded with a DPCM at the byte level) |
| MIFF (X Window)                          | RLE or DPCM                              |
| PIX (SGI IRIS)                           | RLE                                      |
| BW (SGI IRIS)                            | RLE                                      |

| Utility                                  | Compression                         |
| ---------------------------------------- | ----------------------------------- |
| Compress (Unix)                          | LZW                                 |
| [gzip ](http://www.gzip.org/algorithm.txt)(Unix) | A variant of LZ77 (Lempel-Ziv 1977) |

**Comments**: The following graphic file formats do not use compression: PBM, PGM, PPM, PNM, RAS (SUN Raster file format), PCX

[Back to Top](http://www2.seas.gwu.edu/~ayoussef/cs6351/lossless.html#top)

$a^2 = 3$


$$
a^2 = 3
$$








