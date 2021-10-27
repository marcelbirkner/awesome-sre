---
cover: ../.gitbook/assets/Screenshot 2021-10-15 at 09.14.12.png
coverY: 0
---

# Basic Commands

## List all files in directory

```
# show all details
ls -la

# Sort by time modified in reverse lexicographical order
ls -lrt
```

## List all running processes

```
ps aux
```

## Kill running process

```
# Syntax
kill {-signal | -s signal} pid 

# display all the available signals
kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL
 5) SIGTRAP	 6) SIGABRT	 7) SIGEMT	 8) SIGFPE
 9) SIGKILL	10) SIGBUS	11) SIGSEGV	12) SIGSYS
13) SIGPIPE	14) SIGALRM	15) SIGTERM	16) SIGURG
17) SIGSTOP	18) SIGTSTP	19) SIGCONT	20) SIGCHLD
21) SIGTTIN	22) SIGTTOU	23) SIGIO	24) SIGXCPU
25) SIGXFSZ	26) SIGVTALRM	27) SIGPROF	28) SIGWINCH
29) SIGINFO	30) SIGUSR1	31) SIGUSR2

# send SIGKILL to process
kill -9 <pid>
```

## Combine Linux commands using Pipe Symbol |

This is the most powerful feature of linux commands. You can use the output from one command as input for the next using the pipe symbol |. This allows for great ad-hoc slice-and-dicing of data.

```
ps aux | grep "search_string"
ps aux | grep -v "exclude_string"

# get list of processes -> get only the user -> sort output -> count unique occurrences
ps aux | awk '{print $1}' | sort | uniq -c
      1 USER     
    388 marcelbirkner
    169 root
```

### Count lines of code in a directory

```
> cloc .

     872 text files.
     689 unique files.
     297 files ignored.

github.com/AlDanial/cloc v 1.88  T=0.57 s (1018.4 files/s, 47321.6 lines/s)
--------------------------------------------------------------------------------
Language                      files          blank        comment           code
--------------------------------------------------------------------------------
ERB                              82           1607              0          10084
Ruby                            206           1046           1247           6391
Bourne Shell                     17            360            250           1512
JSON                             96            107              0           1358
YAML                             95            176              0            804
Markdown                         67            314              0            758
Bourne Again Shell                9             84             35            260
Python                            1             31             17            208
Lua                               4             60             14            165
Groovy                            1              0              0             10
D                                 1              0              0              5
--------------------------------------------------------------------------------
SUM:                            579           3785           1563          21555
--------------------------------------------------------------------------------
```
