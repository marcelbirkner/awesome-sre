# awk / sed

## awk

Pattern-directed scanning and processing language. Awk scans each input file for lines that match any of a set of patterns specified literally in prog

```
# print 2nd parameter from output for each line
history | awk '{print $2}'

# find 20 most used commands
# * sort: sort output
# * uniq -c: count unique lines
# * sort -n: sort by line count numerical
# * tail: show last 20 results
history | awk '{print $2}' | sort | uniq -c | sort -n | tail -n 20
```

## sed 

The sed utility reads the specified files, or the standard input if no files are specified, modifying the input as specified by a list of commands. The input is then written to the standard output.

```
# replace string in file
sed -i 's/old_string/new_string/g' input.txt

# find string in all files in the current directory and
# replace it in the output with a new string
grep -ri "old_string" . | sed 's/old_string/new_string/g'
```
