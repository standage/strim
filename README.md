# strim: stream Trimmomatic

Using a couple of named pipes and Bash process substitution, it's pretty straightforward to invoke Trimmomatic in a streaming fashion in the shell.

```bash
mkfifo out1
mkfifo out2

java -jar /path/to/trimmomatic.jar -threads 4 -phred33 small-1.fq.gz small-2.fq.gz out1 /dev/null out2 /dev/null MINLEN:40 &
paste <(paste - - - - < out1) <(paste - - - - < out2) | tr '\t' '\n'

wait
rm out1 out2
```

The `strim` script puts a little bit of CLI lipstick on this pig, but the important logic boils down to these 6 commands.

Invoke `strim` like so.

```bash
./strim -t 4 -3 -l in1.fq.gz -r in2.fq.gz /path/to/trimmomatic.jar MINLEN:40 | more | commands | here
```

I'd like to translate this into Python, but would need some assistance with the engineering from someone with a bit more Python experience.
