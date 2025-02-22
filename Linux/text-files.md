# Common Linux Commands for Text Files

## Viewing Files

- `cat filename`: Concatenate and display the content of a file.
- `less filename`: View the content of a file one screen at a time.
- `more filename`: Similar to `less`, but with more limited navigation.
- `head -n N filename`: Display the first N lines of a file.
- `tail -n N filename`: Display the last N lines of a file.

## Editing Files

- `nano filename`: Open a file in the Nano text editor.
- `vim filename`: Open a file in the Vim text editor.
- `sed 's/old/new/g' filename`: Replace all occurrences of 'old' with 'new' in a file.
- `awk '{print $1}' filename`: Print the first column of a file.

## Searching Files

- `grep 'pattern' filename`: Search for a pattern in a file.
- `grep -r 'pattern' directory`: Recursively search for a pattern in a directory.
- `find /path -name "filename"`: Find a file by name in a specified path.

## Manipulating Files

- `sort filename`: Sort the lines in a file.
- `uniq filename`: Remove duplicate lines from a file.
- `wc filename`: Count the number of lines, words, and characters in a file.
- `cut -d 'delimiter' -f N filename`: Extract the Nth field from a file using a delimiter.
- `cut -c N filename`: Extract the Nth character from each line of a file.

## Combining Files

- `paste file1 file2`: Merge lines of files side by side.
- `join file1 file2`: Join lines of two files on a common field.

## File Permissions

- `chmod 644 filename`: Change the permissions of a file to be readable and writable by the owner, and readable by others.
- `chown user:group filename`: Change the owner and group of a file.

## Miscellaneous

- `file filename`: Determine the type of a file.
- `touch filename`: Create an empty file or update the timestamp of an existing file.
