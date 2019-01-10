# Questions

## HPC good citizenship

1. On the UCI cluster, the resource request "-pe openmp 64" refers to the number of processors requested.  Does that
   request mean that your commands will use multiple processors?
   #### Using the command '-pe openmp 64' just reserves 64 processors for your job, it doesn't mean that your job will actually use that many processors. The number of processors used by your job is determined by the actual script and parameters within it.
2. In general, how do you know how many processors, how much RAM, how many files would be required/needed/written by the
   jobs you are running on the cluster?
   #### If you haven't run a job before you can run a test job with sample inputs along with the following line of code before your commands: 
   <pre><code>/usr/bin/time -v</pre></code>
   #### This will output CPU time, RAM, and files required/needed/written requirements for your command.
3. In order to be a "good citizen", you need to have some idea of much RAM your job requires.  In particular, you need
   to know the "peak" (i.e., maximum) RAM required at any point during execution.  Show an example of the shell command
   that you would use on a Linux machine to measure run time and peak ram usage of an arbitrary command, where the time/peak RAM values are written to a file.
   <pre><code>/usr/bin/time -v bedtools bamtobed -i Sample_File.bam > stats.txt </pre></code>
   #### The peak RAM your job uses will show up as Maximum resident set size in your generated text file stats.txt.
4. What are the units of your answer for number 3?
   #### Kilobytes
5. What are the bash commands for the following operations:

    * Checking that a file exists
    <pre><code> find /directory/ -name file_name </pre></code>
    #### If the file exists then you should get an output of /directory/file_name. If it doesn't exist you won't get an output
    * Checking that a file exists and is not empty
    <pre><code> [ -s /directory/file_name ] 
    echo $?  </pre></code>
    #### If the file exists and is not empty, then you will get a zero output, if it's empty then you get a 1 output

6. How would you use the commands from your answer to 5 to write a work flow for HPC that only runs a job if the
   expected output file is **not** present.
   <pre><code> if [ -s /directory/file_name ] 
        then 
            echo "file exists, no need to run script"
        else
            qsub /directory/job.sh
    fi 
</pre></code>

## Trickier questions

7. Would your answer to number 3 work on Apple's OS X operating system?  If no, do you have any idea why not? 
   #### No; Mac OS X doesn't have the GNU command time pre installed in the base operating system. If you wanted to use it you would have to first run the following:
   <pre><code> brew install gnu-time </pre></code>

8. Most of the HPC nodes have 512Gb (gigabytes) of RAM. Let's say you have a job that will require **no more** than 24Gb
   of RAM.  How would you request resources so that you can run more than one job on a node **and** not cause nodes to
   crash?  Show an example of a skeleton HPC script as part of your answer.  **Knowing how to do this is super important
   and will save you loads of frustration and prevent you from taking out your colleagues' jobs on the cluster,
   preventing you from getting nasty emails from Harry!!!!!!!!!!!**
<pre><code>#!/bin/bash
#$ -N Test_script
#$ -q pub64
#$ -pe one-node-mpi 2-64
#$ -R y
#$ -m beas
   </pre></code>
