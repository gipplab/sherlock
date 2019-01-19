Information / Disclaimer
========

Sherlock was written by Rob Pike and Loki. Unfortunately their official website for it went
offline somewhere in 2018 (http://rp-www.cs.usyd.edu.au/~scilect/sherlock/).

This readme is copied from a snapshot of archive.org.
(https://web.archive.org/web/20180219024142/http://web.it.usyd.edu.au/~scilect/sherlock/)

This repository of Sherlock is used by our group and should allow others to reproduce our research. It will turned down as soon as the official website is online again.

# The Sherlock Plagiarism Detector

## What is Sherlock

Sherlock is a program which finds similarities between textual documents. It uses 
digital signatures to find similar pieces of text. A digital signature is a number
which is formed by turning several words in the input into a series of bits and 
joining those bits into a number.

Sherlock works on text files such as essays, computer source code files, and other 
assignments in digital form. It will even work with Tar files, but not compressed
archives such as Gzipped or Zipped files (you'll have to unzip those files first). 

## How do I download it?

Sherlock will internally generate a signature for each file selected and 
compare them against each other and calculate their similarity / overlapping score.

*   [sherlock.c](lib_source/sherlock.c) - Source code file of Sherlock. 
*   [Makefile](lib_source/makefile_sherlock) - Makefile for sig and comp combined.

## How do I compile it?

If using Unix or Linux, just type "make". If using Windows, consult your compiler's 
documentation. You simply need to compile the sherlock.c source code file into an executable called sherlock (sherlock.exe for Windows users). 

## How do I use it?

Sherlock is a command-line program. That is, you run it from an xterm or DOS window. It isn't graphical, and has no graphical user interface. Use it like this:

    sherlock *.txt

That will compare all the text files in the current directory and produce a listing of the most similar files, together with a percentage similarity index.

To compare source files, you might use it like this:

    sherlock *.java

Actually, it's a good idea to redirect the output into a file, so you can examine it in detail. Otherwise it'll just flash past very quickly. To redirect the output into a file, you use the > symbol:

    sherlock *.java > results.txt

This creates a file called "results.txt" which contains the results.

## What options does the program support?

There are several command-line options to Sherlock:

*   <tt>-t _threshold%_</tt> This controls how similar files must be before they will be mentioned. Increase this to 50% or higher if you only want to see very similar files. The default is 20%
*   <tt>-z _zerobits_</tt> This controls the "granularity" of the comparison. The higher this number, the cruder the comparison but the faster it will proceed. The lower this number, the more exact the comparison, but it will be slower, and it may be harder to detect plagiarism because small changes will fool the program into thinking the files are different. The default is 4, but the number can range from 0 to 31.
*   <tt>-n _number_of_words_</tt> This controls how many words are used to form one digital signature. This also contributes to the granularity of the comparison. A higher number is slower while a lower number is less exact. The default is 3 words, which works fine in most cases.
*   <tt>-o _outfile_</tt> If using Windows it may be difficult to specify an output file on the command line. Use this option to specify the output file.

Examples:

<pre>
   sherlock -t 80% -z 3 -n 2 -o results.txt *.java
   sherlock -t 50% -o results.txt *.txt
   sherlock -t 0% *.java   # reports all similarity indexes
</pre>

## What does the output look like?

Sherlock performs an N<sup>2</sup> comparison between all the files, so every file is compared with every other file.

The output lists the similarity indexes between each pair of files. This index is a percentage, where 0% means no similarity and 100% means there is a very high chance of a lot of similarity. 100% does not mean that the files are exactly the same, since the Sherlock program randomly throws away some data in order to perform a faster match.

The output of the program might look like this:

<pre>
   README and index.html: 5%
   README and makefile: 1%
   README and sherlock: 0%
   README and sherlock.c: 2%
   index.html and makefile: 8%
   index.html and sherlock: 0%
   index.html and sherlock.c: 10%
   makefile and sherlock: 0%
   makefile and sherlock.c: 6%
   sherlock and sherlock.c: 0%
</pre>

(The threshold is normally 20%, so the above output would not ordinarily be shown. The example used a threshold lowered to 0% in order to see all similarities. The numbers are fake, however, and just for illustrational purposes.)

In the example, the most similar files are index.html and sherlock.c with a rating of 10%. This means that approximately 10% of the material in those two files might be overlapping (i.e. appear in both files).

## Credits

Rob Pike created the original version of this program. There were actually two programs, 
called <tt>sig</tt> and <tt>comp</tt>. The <tt>sig</tt> program generated the digital 
signatures and stored them in a file. The <tt>comp</tt> program would then be used to
 ompare the signature files and report the similarities.

Loki combined the two programs into a single program, called Sherlock. This has some 
advantages and disadvantages.

The main advantage is that no intermediate files need be created. Intermediate files 
require disk space and a management strategy. For example, you need to decide what 
suffix the signature files will use (e.g. ".sig") and where they will be stored (with the data, in a parallel directory structure, or other place).

Sherlock avoids these issues but requires all files to be compared every time to want 
a comparison. For example, if you are looking for similarities in essays handed in by students, Sherlock may work well because you should have all the essays handed in before you begin the plagiarism detection.

On the other hand, Sherlock is not see well suited to detecting duplicate email messages,
since email arrives continually, and you can never have a finished set of email to work
on. Intermediate ".sig" files might reduce the time needed to compare a new email 
item to older items, since the signatures for old items will already be computed and
stored. Sherlock would have to read and compute the signatures for all of those older 
files, every time you need a comparison, because it does not use ".sig" files.

## Can I download sig and comp?

sig will generate a single signature file.

*   [sherlock_sig.c](lib_source/sherlock_sig.c) - sig will generate a single signature file.
*   [sherlock_comp.c](lib_source/sherlock_comp.c) - comp will compare any number of signature files against each other and calculate their similarity / overlapping score.
*   [Makefile](lib_source/makefile_sherlock_sig_comp) - Makefile for sig and comp combined.


# Signature and Comparison (Additional Readme)

## Usage sig - Signature

    sherlock_signature source.txt
    sherlock_signature source.txt > signature_file.txt      # or directly write the output into a file

You can use the <tt>-z _zerobits_</tt>, <tt>-n _number_of_words_</tt> and <tt>-o _outfile_</tt> option.

## Usage comp - Compare

    sherlock_compare signature_file_1.txt signature_file_2.txt   # to compare 2 files directly
    sherlock_compare *.txt > result.txt                      # compare any files and write output into a result file.

You can use the <tt>-t _threshold%_</tt> option.

Output is the same a the normal Sherlock program.
