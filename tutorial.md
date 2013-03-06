bash in a Unix 'shell,' a command-line interpreter that allows you to enter
commands that the computer can then run. There are many other shell programs
available, but bash is widely used and in many cases the default shell.

The shell also allows you to manipulate files: create, edit, move, or delete
them. Files are organized into directories (sometimes called folders in systems
with a user interface, such as Windows). Files have a specific location on your
hard drive, called a path, that can be specified by listing the sequence of
directories needed to get there.

An absolute path starts from the root directory, whereas a relative path starts
from the current working directory. The root directory always starts with a
single slash. To print the absolute path of your working directory:

  pwd

When you start a new shell, your working directory will be your home directory.
When you want to refer to a file, you don't always have to specify the absolute
path of a file. You can use a path relative to your working directory.

To see an example, first list the files and directories contained in your
current working directory:

  ls

Pick any file or directory and print more information about it:

  ls -l [relative_path]

You didn't need to write down the absolute path because files and directories
can be specified by a relative path (relative to the working directory).


ls [directory]  show files in that directory
ls [pattern]    show files that match the pattern
  examples of [pattern] *, m*, ~/*

cd [dir]    change directory
cd ..       change to parent directory
cd          change to home directory


mkdir    create new directory



***
Task: Create the following directory/file structure.

+ project
  + analysis
    - results (do not create yet)
  + data
    - m-0.0
    - m-0.1
    - m-0.5

In each data file, enter 5 random numbers (fitness values).
***



If you ever want to cancel the command you're typing, you can press Ctrl-C.  If
you want to re-enter a previous command, press the up arrow key until you find
it. You can modify the command. To see a history of commands you have entered,
use the program called 'history'.



cp       copies a file to another
cp -r    copies a directory and its contents to another directory

rm       deletes a file
rm -rf   deletes a directory/file and all its contants (warning: dangerous)

mv    moves (or renames) a file or directory



cat    show and concatenate contents of files



***
Task: Show contents of all data files.
***


25 min.


head         show the first 10 lines of a file
head -n 5    show the first 5 lines of a file

tail         show the last 10 lines of a file
tail -n 5    show the last 5 lines of a file



By default, programs output their results to the screen, but in many cases you
may want to save that output to a file.  You may do this by using the greater
than symbol ('>') to redirect the 'standard output' of a program to a file.

For example, to save the first two lines of a file to a new file:

  head -n 2 [path] > new_file

In addition to the standard output, there is also the standard input, which
many programs may use to obtain information to operate on.  By default, the
standard input is the command-line terminal itself, but it is often useful to
make the standard output of a program be the standard input of another.

For example, to print out the first two lines of a file, we said:

  head -n 2 [path]

But we could have equally have written this as:

  cat [path] | head -n 2

The 'pipe' symbol causes the output of the cat program to become the input of
the head program. Notice that we did not specify a file to the head
program---this tells the head program to operate on its standard input.

Note that you can continue to redirect the output as many times as you want,
simply adding the pipe symbol followed by the name of the program.  This
ability to redirect output is very useful when you are processing data, each
time doing something to it---it is a data pipeline.



***
Task: Print out the third line of a file.
Possible answer: cat [file] | head -n 3 | tail -n 1
***



If you ever need help with a particular command:

  man [command]



Like in most programming languages, you can assign a value to a variable to
store it in memory. For example, to assign a file name to the variable path:

  path=data/m-0.0

We can then use this variable in a command:

  cat $path

You can use the program echo to see what a variable contains; echo simply
prints out whatever text you give it to the standard output:

  echo $path

Also like in most programming languages, you may create a loop to repeat some
command. A 'for' loop assigns a variable each value after the 'in' keyword:

  for n in 1 2 3; do echo $n; done

This simple loop prints out each value per line. The list of values can be any
list of text:

  for n in red green blue; do echo $n; done



***
Task: Using a for loop, print out the name of each data file.
Possible answer: for f in m-0.0 m-0.1 m-0.5; do echo $f; done
***


45 min.


The '$' operator is quite interesting: not only can you access the value of a
variable using it, but you can access the output of an entire command.  For
example, the following command assigns the first line of a file to a variable:

  first_line=$(head -n 1 data/m-0.0)
  echo $first_line



***
Task: In the above for loop, instead of typing each file, use the $() operator.
Possible answer: for f in $(ls project/data); do echo $f; done
***



The paste command allows you to concatenate data by columns.  For example, try:

  paste data/m-0.0 data/m-0.1 data/m-0.5

What happened? The lines of each file were joined by columns, separated by tabs
(the default). Use the -d' ' option to separate by spaces. The paste command
may also take in standard input, which is specified by a '-':

  cat data/m-0.1 | paste data/m-0.0 - data/m-0.5



Let's put together some of the things we've learned to solve a common task:
re-structure raw data into one file that is easier to analyze.  For example, we
would like to have one results file that contains three columns: the mutation
rate (our treatment), the replicate number (1-5), and the fitness.

  MutRate Replicate Fitness
  0.0 1 4.3
  0.0 2 3.5
  0.0 3 2.1
  ...
  0.5 5 7.2

We have three columns, which we know can be created using paste, if we can get
each of the columns separately.



***
Task: Generate the first column.
Possible answer:
  for m in 0.0 0.1 0.5; do for r in 1 2 3 4 5; do echo $m; done; done > col_1
***



***
Task: Generate the second column.
Possible answer:
  for m in 1 2 3; do for r in 1 2 3 4 5; do echo $r; done; done > col_2
***



***
Task: Generate the third column.
Possible answer:
  cat data/m-0.0 data/m-0.1 data/m-0.5 > col_3
***



We can put it all together:

  paste -d' ' col_1 col_2 col_3 > analysis/results


65 min.


Sometimes you want to extract data columns from a file. Use the cut command:

  cut -d' ' -f2 analysis/results

The sort command sorts lines of text. Use the -r option to sort in reverse
and/or the -n option to sort numbers rather than text.



***
Task: Output the largest number from a list of numbers given by standard input.
Possible answer: cat [path] | sort -nr | head -n 1
***



Suppose that you did not know that the number of lines in each data file was 5.
A quick way to find out the number of lines in a text file is to use the wc
command (wc means 'word count'). But it doesn't just count words. It can also
count lines and individual characters.

To count lines:

  wc -l [file] or cat [file] | wc -l

Suppose that each data file contained one hundred lines, and you wanted to
re-structure the data as we did before. You don't want to list the numbers
1-100 in the for loop. The seq command generates a list of numbers in the range
that you specify.

For example, the following command outputs a list from 1 to 100:

  seq 100



***
Task: Re-do the re-structuring task by not assuming a fixed length of data.
  Assume that all the data files will have the same length.
Possible answer:
  length=$(wc -l data/m-0.0)
  num_list=$(seq $length)
  for m in 0.0 0.1 0.5; do for r in $num_list; do echo $m; done; done > col_1
  for m in 1 2 3; do for r in $num_list; do echo $r; done; done > col_2
  cat data/m-0.0 data/m-0.1 data/m-0.5 > col_3
  paste -d' ' col_1 col_2 col_3 > analysis/results
***


80 min.


So far, the scripts you have written you have run directly on the command line.
Sometimes you will want to save your script to a file so that you can run
later. This is especially important for scripts that span multiple lines.  To
create a script file and run it, open a new file and type in the script, add
the line #!/bin/bash as the first line, and then change the file mode:

  chmod +x [script]

Now you can run the script:

  ./[script]



***
Task: Create a script with the previous exercise and run it.
***



Other useful commands not covered:

scp
tr
which
if, then, fi
while read line
uniq
grep
ssh


90 min.
