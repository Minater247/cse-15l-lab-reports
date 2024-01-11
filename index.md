Hello, World! This is the home of my lab reports for CSE 15L.

<details>
  <summary>Lab Report 1 - Remote Access and Filesystem</summary>

  # `cd`
### No Arguments:
  - <img width="447" alt="Screenshot 2024-01-11 130559" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/f8eff158-274a-4b4f-98c9-f7b4fc86b953">

  - Working directory at runtime: `/home` (ended at `/home`)
  - The command was not given any arguments, and therefore made no changes and simply returned without output.
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
  
  # `ls`
### No Arguments:
  - <img width="135" alt="Screenshot 2024-01-11 125501" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/275cb881-3685-45f7-bd57-8dd34816b53f">

  - Working directory at runtime: `/home`
  - The command printed a list of the directories within the working directory. In this case, I have only cloned a single directory, which is present as lecture1.
  - The output is not an error.
    <br>
### Path to Directory:
  - <img width="395" alt="Screenshot 2024-01-11 125506" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/c9acf49c-e282-4c6e-ae17-a9fc8e98508e">

  - Working directory at runtime: `/home`
  - The command printed the contents of the given directory (and will print all directories given, if multiple, separated by spaces). In this case, various system directories are present at the root, such as `/bin` (executables), `/mnt` (drives) and `/dev` (devices).
  - The output is not an error.
    <br>
### Path to File:
  - <img width="269" alt="Screenshot 2024-01-11 125512" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/a6d69c9c-bbc0-40cb-a88e-7a5695366493">

  - Working directory at runtime: `/home`
  - The command printed the path to the file. It did not print the contents of the file, or any information about the file.
  - The output is not an error.
  
  # CAT
  <br>
  No Arguments:
  -
  -
  -
  -
  Path to Directory:
  -
  -
  -
  -
  Path to File:
  -
  -
  -
  -
</details>
