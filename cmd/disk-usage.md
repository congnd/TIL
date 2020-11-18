## Get the summarized sizes of subdirectories
```
sudo du -sh ./* | sort -h | tail -n 10
```

What this means:

-s to give only the total for each command line argument.
-h for human-readable suffixes like M for megabytes and G for gigabytes (optional).
./* simply expands to all directories (and files) in the current directory.

Source:
https://superuser.com/questions/162749/how-to-get-the-summarized-sizes-of-directories-and-their-subdirectories
