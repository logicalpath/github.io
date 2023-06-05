---
layout: post
title: "Reading files in a bash script"
---

The code below reads all the files in /data/logs subdirectory under the directory where the script is executed `pwd`. For each `file` in the logs directory, the script prints the name of the file. The `while` loop reads each line of the file and prints the line. 

```shell
    dir=`pwd`/data/logs
    for file in "$dir"/*
    do
      echo "$file"
      while read -r line
      do
        printf '%s\n' "$line"
      done < "$file"
    done
```





