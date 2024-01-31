Hello, World! This is the home of my lab reports for CSE 15L.


# Lab Report 1 - Remote Access and Filesystem

## `cd`

### No Arguments:

- <img width="158" alt="Screenshot 2024-01-30 162800" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/762d78e4-1a4e-454d-b6ed-6e7274eb7ca4">

- Working directory at runtime: `/mnt/` (ended at `/home`)
  
- Given no arguments, the command returned the working directory to the user's home directory: `/home`, in this case.
  
- The output is not an error.
  
### Path to Directory:

- <img width="149" alt="Screenshot 2024-01-11 130631" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/e024b8e1-01b2-46fc-8948-091aeae8ce72">

- Working directory at runtime: `/` (ended at `/home`)
  
- The command took the directory and expanded the home marker `~` to user `sahara`'s home directory, `/home`. It then changed the working directory to that location, and did not output anything - the only change in view being that the next terminal prompt had the directory as `~` rather than `/`.
  
- The output is not an error.
  
### Path to File:

- <img width="317" alt="Screenshot 2024-01-11 130636" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/c461d977-faa0-48f4-85b6-b00fb406d45f">

- Working directory at runtime: `/home`
  
- The command took the file, saw that it was not a directory, and knew this was not possible. It proceeded to output that it could not continue.
  
- The output was an error, as `bash` (the shell) reported that `cd` (the command) failed to set the working directory (which must be a directory) to a non-directory file descriptor.

## `ls`

### No Arguments:

- <img width="135" alt="Screenshot 2024-01-11 125501" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/275cb881-3685-45f7-bd57-8dd34816b53f">

- Working directory at runtime: `/home`
  
- The command printed a list of the directories within the working directory. In this case, I have only cloned a single directory, which is present as lecture1.
  
- The output is not an error.
  
  
  
### Path to Directory:

- <img width="395" alt="Screenshot 2024-01-11 125506" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/c9acf49c-e282-4c6e-ae17-a9fc8e98508e">

- Working directory at runtime: `/home`
  
- The command printed the contents of the given directory (and will print all directories given, if multiple, separated by spaces). In this case, various system directories are present at the root, such as `/bin` (executables), `/mnt` (drives) and `/dev` (devices).
  
- The output is not an error.
  
### Path to File:
- <img width="269" alt="Screenshot 2024-01-11 125512" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/a6d69c9c-bbc0-40cb-a88e-7a5695366493">

- Working directory at runtime: `/home`
- The command printed the path to the file. It did not print the contents of the file, or any information about the file.
- The output is not an error.

## `cat`
### No Arguments:
- <img width="168" alt="Screenshot 2024-01-11 131449" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/966dfc0a-2454-4b4e-a62a-0f04ab80826b">

- Working directory at runtime: `/home`
- The command tries to read from the stream `stdin`, which in this case points to the terminal. All characters typed are echoed when a newline is given, as I'm guessing the internal representation when reading this is using some form of readLine().
- The output is not an error.
### Path to Directory:
- ![image](https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/0b55fed0-7d8e-492a-ada9-a59f80069866)

- Working directory at runtime: `/home`
- The command failed to read the contents of the given file descriptor, as it pointed to a directory and was invalid. This would be an application of `ls`, which would internally use `fopendir` rather than `fopen`, which would properly read a directory.
- The output is an error, as it did not output any contents and simply said that it was a directory (which is invalid input for `cat`.
### Path to File:
- <img width="455" alt="Screenshot 2024-01-11 131518" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/7fb7c1bf-026a-4819-8e29-735c4acd1bc2">

- Working directory at runtime: `/home`
- The command output the contents of the file. Notably, without the trailing newline flag, the shell prefix showed up on the same line as the last bracket!
- The output is not an error.
