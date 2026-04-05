# Bandit Level 0                                                       

## **Goal**
* Log into the server using SSH with the given credentials

## **Explanation**
* This level introduces **SSH (Secure Shell)**, a protocol used to securely connect to remote systems.
* You are provided with:
  * Host: `bandit.labs.overthewire.org`
  * Port: `2220`
  * Username: `bandit0`
  * Password: `bandit0`

## **Solution**
* Run the following command:

ssh bandit0@bandit.labs.overthewire.org -p 2220

---

# Bandit Level 0 → Level 1

## **Goal**
* Find the password for the next level in the file `readme` located in the home directory.
* Use this password to log into **bandit1** using SSH on port 2220.

## **Explanation**
* The file `readme` is a simple text file in the home directory of **bandit0**.
* Whenever you find a password for a level, you should use it to SSH into the next level.
* This level teaches you basic file reading and SSH login to progress through the game

## **Solution**
* First we check as a good practice , the directory in which we are in using `pwd` , we see that we are in the `/home/bandit0`.
* Then we do `ls -l` to check the files in the user's home directory , we find the `readme` file and we do `cat readme` to open it and get the key.
* Key: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

---

# Bandit Level 1 → Level 2

## **Goal**
* Find the password in the file named `-`

## **Explanation**
* This level is tricky because running `cat -` doesn’t work as expected.
* In Linux, `-` is treated as a special argument representing **standard input (stdin)**, not a regular filename.
* You need to tell the shell explicitly that `-` is a file in the current directory.

## **Solution**
* Use the relative path `./-` to read the password: `cat ./-`
* Doing so we receive the next key: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

---

# Bandit Level 2 → Level 3

## **Goal**
* Find the password in the file named `--spaces in this filename--` located in the home directory.

## **Explanation**
* This level is tricky because the filename **starts with `-`** and contains spaces.
* In Linux, a filename starting with `-` is treated as a special argument (like stdin) if you don’t specify the path.
* Spaces in filenames also need to be handled properly so the shell interprets the name as a single file.
* To solve this, we need to combine what we've learned in the previous level while using our own knowledge:
  * **Relative path `./`** tells the shell it’s a file in the current directory
  * **Quotes `" "`**  preserve spaces in the filename

## **Solution**
* Read the password from the file using: `cat "./--spaces in this filename--"`
* Doing so we've received the key: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

# Bandit Level 3 → Level 4

## **Goal**
* Find the password for the next level, which is stored in a **hidden file** inside the `inhere` directory.

## **Explanation**
* Hidden files in Linux start with a `.` and are not shown by default in directory listings.  
* To access them, you need to **navigate into the directory** and then **list files including hidden ones**.  
* This level teaches basic navigation and working with hidden files.

## **Solution**
* Change into the `inhere` directory: cd inhere
* After that we must do `ls -la` to find the hidden file "...Hiding-From-You" , only then we can do `cat ...Hiding-From-You`. 
* Doing so we've received the key: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

# Bandit Level 4 → Level 5

## **Goal**
* Find the password for the next level, which is stored in the **only human-readable file** in the `inhere` directory.

## **Explanation**
* The `inhere` directory contains many files with names like `-file00`, `-file01`, etc.  
* Most of these files are **binary** and reading them directly with `cat` will fill your terminal with garbage data , so trying random files would mess up with your terminal.  
* The `file` command checks a file's **magic bytes** and tells you its type.  
* You can use `file` to find the single **ASCII text** file that contains the password.

## **Solution**
* Change into the `inhere` directory: `cd inhere`
* After that we do `ls -l` and we see many files , but none really stand out , so we must do `file ./*` to find the human-readable (ASCII text) file. The file was `-file07`.
* Only thing left to do now is to do `cat ./-file07` and done.
* Thus we now have our key: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

