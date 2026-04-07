# Bandit Level 5 → Level 6

## **Goal**
* Find the password for the next level. The file is located somewhere under the `inhere` directory and has the following properties:
  * human-readable  
  * exactly **1033 bytes** in size  
  * **not executable**

## **Explanation**
* The `inhere` directory contains many subdirectories and files.  
* Manually checking each file would be inefficient.  
* The `find` command allows us to search for files based on specific criteria such as size, type, and permissions.  
* This level teaches efficient file searching using filters.
* The knowledge learned in this level is highly important for being as efficient as possible in searching.

## **Solution**

### Method 1 (Efficient - recommended)
* Use `find` with filters: `find . -type f -size 1033c ! -executable`
* Using `-type f` we already reduce the search to only files
* The filter for finding a file of **1033 bytes** is `-size 1033c`
* Now to make sure it is **NOT** executable we do the negation `!` before `-executable`
* After running the command we find that our file is located in: **./maybehere07/.file2**
* The key found : [Redacted]

---

# Bandit Level 6 → Level 7

## **Goal**
* Find the password for the next level. The file is located somewhere on the server and has the following properties:
  * owned by user **bandit7**  
  * owned by group **bandit6**  
  * exactly **33 bytes** in size  

## **Explanation**
* At this level, I realized the file is no longer in a specific folder like `inhere`, so I have to search the entire system.  
* Doing this manually would be impossible, so I needed a better way.  
* That’s where the `find` command becomes really useful.  

* I learned that `find` can filter files based on:
  * who owns them  
  * what group they belong to  
  * their size

* Since the file has very specific properties, I can combine all these filters to narrow it down to a single result.  
* While searching from `/`, I also noticed a lot of “Permission denied” messages.  
* These aren’t useful, so I decided to hide them to keep the output clean. ( **2>/dev/null** )

## **Solution**
* Use `find` with all required filters: `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null`
* In return we were given the correct path to the file: **/var/lib/dpkg/info/bandit7.password** , after using cat we got the right key.
* The key for the next level is : [Redacted]

---

# Bandit Level 7 → Level 8

## **Goal**
* Find the password for the next level. The password is stored in the file `data.txt` next to the word **millionth**.

## **Explanation**
* The file `data.txt` is readable, but it’s very large, so opening it normally and scrolling would be inefficient.  
* My first instinct was to use the `grep` command, but I also thought that a text editor with a built-in search feature could work just as well.  
* I decided to try both methods, and I also wanted to open the file to see how the data inside is structured.
* Inside `vim`, you can search for a word using `/keyword`, which makes it much easier to locate specific content in large files.

## **Solution**

### Method 1 ( Using grep )
* We just do : `grep "millionth" data.txt`
* The single line containing that word will be printed.
### Method 2 (Using vim)
* Open the file: `vim data.txt`
* Search for the keyword using `/millionth` inside vim.
* You will see among other lines , the line we've been searching for **millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc**
* The key to the next level: [Redacted]

---

# Bandit Level 8 → Level 9

## **Goal**
* Find the password for the next level. The password is stored in the file `data.txt` and is the **only line that occurs once**.

## **Explanation**
* The file contains many repeated lines, so manually checking would be inefficient.  
* I realized I need a way to group identical lines together then to filter them out.  
* From what I learned, the `uniq` command can detect unique or repeated lines, but it only works properly if the file is sorted first.  
* So the idea is to combine `sort` and `uniq`.

## **Solution**
* Sort the file and find the unique line: `sort data.txt | uniq -u`
* Searching the manual i found out about the  **-u** flag that lets me only get back the words that appear only once.
* After running the command I've received successfully the key to the next level: [Redacted]

---

# Bandit Level 9 → Level 10

## **Goal**
* Find the password for the next level. The password is stored in the file `data.txt`, inside one of the few human-readable strings, preceded by several `=` characters.

## **Explanation**
* When I first looked at `data.txt`, it didn’t look like normal text — it seemed to contain a lot of unreadable (binary) data.  
* So using `man` on the **strings** command helped me research a bit on its uses.
* I learned that the `strings` command can extract **human-readable text** from binary files.  
* That makes it much easier to filter out only the readable parts.  
* Since the password is preceded by multiple `=` characters, I can combine `strings` with `grep` to narrow it down.

## **Solution**
* Extract readable strings and filter the relevant line: `strings data.txt | grep "="` 
* Doing so couple of lines were printed containing "=" , but knowing the password format it was easy to pick the right one.
* Thus the password is: [Redacted]

---

# Bandit Level 10 → Level 11

## **Goal**
* Find the password for the next level. The password is stored in the file `data.txt`, which contains **base64-encoded data**.

## **Explanation**
* When I first looked at `data.txt`, it didn’t seem readable — it was all encoded.  
* I remembered `strings` from the previous level and thought maybe I could use it here.  
* I checked `strings --help` to see if there were options like `-e` (encoding) or `-t` (radix) that could help, but they only mentioned **32-bit encoding** or **base 16** output. So that didn’t directly help with base64.  

* Then I saw the suggested commands `base64` and i used the `man base64` and `base64 --help`. 
* Knowing that the content is actually just **standard base64 text**, I could simply **decode it** using the `base64` command.

## **Solution**
* I decoded the file , using the long format of the `-d` flag (`--decode`) just for clarity purposes: `base64 --decode data.txt`
* The following line was printed out: **The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr**
* Meaning that the key for the next level is: [Redacted]  
