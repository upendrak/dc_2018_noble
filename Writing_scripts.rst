**Writing Scripts**
===================

Learning Objectives
-------------------

-  Capture previous commands into a script to re-run in one single
   command
-  Understanding variables and storing information
-  Learn how to use variables to operate on multiple files

Now that you’ve been introduced to a number of commands to interrogate
your data, wouldn’t it be great if you could do this for each set of
data that comes in, without having to manually re-type the commands?

Welcome to the beauty and purpose of shell scripts.

Shell scripts
-------------

Shell scripts are text files that contain commands we want to run. As
with any file, you can give a shell script any name and usually have the
extension ``.sh``. For historical reasons, a bunch of commands saved in
a file is usually called a shell script, but make no mistake, this is
actually a small program.

We are going to take the commands we repeat frequently and save them
into a file so that we can **re-run all those operations** again later
by typing **one single command**. Let’s write a shell script that will
do two things:

1. Tell us what is our current working directory
2. Lists the contents of the directory

First let’s move into the ``/scratch/dc_sample_data`` directory and create a
new file using ``nano``:

::

   $ cd /scratch/dc_sample_data
   $ nano listing.sh

Type in the following lines in the ``listing.sh`` file:

::

   echo "Your current working directory is:"
   pwd
   echo "These are the contents of this directory:"
   ls -l 

..

   The ``echo`` command is a utility for writing to standard output. By
   providing text in quotations after the command we indicated what it
   is we wanted written

Save the file and exit ``nano``. Now let’s run the new script we have
created. To run a shell script you usually use the ``bash`` or ``sh``
command.

::

   $ sh listing.sh

..

   Did it work like you expected?

   Were the ``echo`` commands helpful in letting you know what came
   next?

This is a very simple shell script. Let's write some interesting ones.

One thing we will commonly want to do with sequencing results is pull out bad reads and write them to a file to see if we can figure out what’s going on with them. We’re going to look for reads with long sequences of N’s like we did before, but now we’re going to write a script, so we can run it each time we get new sequences, rather than type the code in by hand each time.

Bad reads have a lot of N’s, so we’re going to look for NNNNNNNNNN with grep. We want the whole FASTQ record, so we’re also going to get the one line above the sequence and the two lines below. We also want to look in all the files that end with .fastq, so we’re going to use the * wild card.

.. code :: bash

  $ grep -B1 -A2 NNNNNNNNNN *.fastq > scripted_bad_reads.txt

We’re going to create a new file to put this command in. We’ll call it ``bad-reads-script.sh``. The ``sh`` isn’t required, but using that extension tells us that it’s a shell script.

.. code :: bash

  $ nano bad-reads-script.sh

Type your grep command into the file and save it as before.

Now comes the neat part. We can run this script. Type:

.. code :: bash

  $ bash bad-reads-script.sh

It will look like nothing happened, but now if you look at ``scripted_bad_reads.txt``, you can see that there are now reads in the file.

-------------------
** Exercise **

How many bad reads are there in the two FASTQ files combined?

Bonus: How many bad reads are in each of the two FASTQ files? (Hint: You will need to use the cut command with the -d flag.)

We want the script to tell us when it’s done.

Open ``bad-reads-script.sh`` and add the line echo "Script finished!" after the grep command and save the file.

Run the updated script.
--------------------

Making the script into a program
--------------------------------

We had to type ``sh`` or ``bash`` because we needed to tell the computer what program to use to run this script. Instead we can turn this script into its own program. We need to tell it that it’s a program by making it executable. We can do this by changing the file permissions. We talked about permissions in an earlier episode.

First, let’s look at the current permissions.

.. code :: bash

  $ ls -l bad-reads-script.sh
  -rw-r--r-- 1 upendra_35 iplant-everyone 57 Jun  4 20:05 bad-read-script.sh

We see that it says ``-rw-r--r--``. This shows that the file can be read by any user and written to by the file owner (you). We want to change these permissions so that the file can be executed as a program. We use the command ``chmod`` like we did earlier when we removed write permissions. Here we are adding (+) executable permissions (+x).

.. code :: bash

  $ chmod +x bad-reads-script.sh

Now let’s look at the permissions again.

.. code :: bash

  $ ls -l bad-reads-script.sh
  -rwxr-xr-x 1 upendra_35 iplant-everyone 57 Jun  4 20:05 bad-read-script.sh

Now we see that it says ``-rwxr-xr-x``. The x’s that are there now tell us we can run it as a program. So, let’s try it! We’ll need to put ``./`` at the beginning so the computer knows to look here in this directory for the program.

.. code :: bash

  $ ./bad-reads-script.sh

The script should run the same way as before, but now we’ve created our very own computer program!
