https://static-bcrf.biochem.wisc.edu/tutorials/docker/Docker_03.html
4.4 clustalomega container

We can now run clustalomega from a container. The example given on the web page adds the name of the files on the docker run command itself:

Run command.

cd $HOME/dockershare
docker run -it --rm -v $HOME/dockershare:/data pegi3s/clustalomega -i /data/sequences.fasta -o /data/sequences_aligned.fasta

After completing the task the container exists and we are back to the host computer prompt.

We e can type the alignment file on the screen:

cat sequences_aligned.fasta

>GIP
YAEGTFISDYSIAMDKIRQQDFVNWLLAQ----
>GLP-1
HAEGTFTSDVSSYLEGQAAKEFIAWLVKGRG--
>GLP-2
HADGSFSDEMNTILDNLAARDFINWLIQTKITD
>glucagon
HSQGTFTSDYSKYLDSRRAQDFVQWLMNT----

The - represent the gaps. Therefore it worked.

However, if you re-run the same command (using the same file names) you’ll get this error:

FATAL: Cowardly refusing to overwrite already existing file
'/data/sequences_aligned.fasta'. Use --force to force overwriting.

Therefore we learn that adding --force will fix that problem.

Other errors might suggest to look into the help:

For more information try: clustalo --help

The help page is rather long, but we can concentrate on the output file and format information:

Alignment Output:
  -o, --out, --outfile={file,-} Multiple sequence alignment output file (default: stdout)
  --outfmt={a2m=fa[sta],clu[stal],msf,phy[lip],selex,st[ockholm],vie[nna]}
  MSA output file format (default: fasta)

We can re-reun the command with a different format which provides a better visual of an alignment. This would be true for the subset clu[stal],msf,phy[lip],selex.

EXERCISE:
Time permitting you can rerun the commands with one or more of these formats.
Do not forget to add --force to overwrite the output file.

For example: (written with line continuation mark \)

docker run -it --rm \
-v $HOME/dockershare:/data pegi3s/clustalomega \
-i /data/sequences.fasta -o \
/data/sequences_aligned.fasta \
--force \
--outfmt=clu

# check output
cat sequences_aligned.fasta

CLUSTAL O(1.2.4) multiple sequence alignment


GIP           YAEGTFISDYSIAMDKIRQQDFVNWLLAQ----
GLP-1         HAEGTFTSDVSSYLEGQAAKEFIAWLVKGRG--
GLP-2         HADGSFSDEMNTILDNLAARDFINWLIQTKITD
glucagon      HSQGTFTSDYSKYLDSRRAQDFVQWLMNT----
              :::*:* .: .  ::    ::*: **:      

EXERCISE 2:
Time permitting create a file (named e.g. sequences2.fasta for longer protein sequence files as detailed in APPENDIX A for “Protein FASTA for clustalomega.”)

Note: There is very little difference between these files, and using the clu format for the output will make the results easier to read.
