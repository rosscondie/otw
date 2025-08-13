# OverTheWire Bandit 

### Level 0 -> Level 1
- password needed: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

### Level 1 -> Level 2
- password needed: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx
  - this was also done using a `-` as a filename so you can cat this with `./-` to see the contents. also using vim to open the file worked.

### Level 2 -> Level 3
- password needed: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
  - this was about escaping spaces in filenames like --space in the filename-- and how to use quotes or just use tab to autocomplete.

### Level 3 -> Level 4
- password needed: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
  - this was looking for the password that was stored in a hidden file
  - using ls with the flag `-alps` allows us to:
    - `-a` show all files, including hidden ones (those starting with `.`)
    - `-l` use long listing format (permissions, owner, size, date etc...)
    - `p` append a `/` to directory names so they are easy to spot
    - `s` show file sizes in blocks (1 block = 512 bytes but can vary)

### Level 4 -> 5
- password needed: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
  - I went through everyfile by using vim and checking which one was human readable but I saw a solution which used `find . -type f | xargs file`

### Level 5 -> 6
- password needed: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
  - The file we were looking for was to be 1033 bytes, not executable and human readable. Using the find command we could `find . -type f ! -executable -size 1033c` which gave us ./maybehere007/.file2 which we then used cat to see the password.

### Level 6 -> 7
- password needed: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
  - The file to find was found by using `find / -type f -user bandit7 -group bandit6 -size 33c` which gave us a lot of files with the message Permission denied. 
  - The `find /` searches "somewhere on the server" the `/` meaning the parent directory or root. 
  - We can then use `2>/dev/null` to redirect the all the errors and such which will show us our password file.
  
### Level 7 -> 8
- password needed: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
  - The password was stored in data.txt file next to the word "millionth" where we ran `grep "millionth" data.txt` to search for it. 
  - Another way would be to redirect or pipe the output from `cat data.txt | grep "millionth` which would output our password.

### Level 8 -> 9 
- password needed: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
  - The password was stored in a data.txt and is the only line of text that occurs only once. 
  - This was found using the `sort` and `uniq` commands by `sort data.txt | uniq -u`which sorted the data.txt file and found the only occurence of the string.

### Level 9 -> 10
- password needed: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
  - `strings` is a command we can use to find the human readable strings from a binary file. We know that the string is preceded by several "=" characters so:
  `strings data.txt | grep =` gives us the strings preceded by the "=" characters.

### Level 10 -> 11
- password needed: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
  - The password was stored in a data.txt file that contained base64 encoded data.
  - We used `base64 -d` to decode the data.txt file `base 64 -d data.txt` to give us the output of the password.

### Level 11 -> 12
- password needed: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4 
  - The password was stored in data.txt file where all lowercase (a-z) and uppercase (A-Z) letters were rotated by 13 positions (ROT 13).
  - `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m` worked to rotate them and show us the password

### Level 12 -> 13
- password needed: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
  - Using `xxd` allows us to make a hexdump or using the flag `-r` do a reverse hexdump. Performing a reverse of the hexdump and get the output of that particular file. 
  - `xxd -r data.txt > data` output the information into another file. 
  - using `file data` this allows us to see the file type of data which shows us it was gzip compressed data. 
  - We need to change the file extension before decompressing with `mv data data.gz`
  - Then we can run `gzip -d data.gz` (-d decompress) and check the files available with `ls`
  - With `file $(filename)` we can then check again the type and repeat the process until we have decompressed the file. It takes a while as it was compressed a lot.
  - We also seen the `.tar` file extension which we can use `tar xf $(filename).tar
  - Ultimately every time we want to decompress the file with a utility we must rename the file to be compatible e.g. `.tar/.bz2/.gz`

### Level 13 -> 14
- password needed: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
  - There was a couple of interesting ways to do this. The ssh key was found in the home directory of bandit13@bandit called `sshkey.private` which you could then use to ssh into the bandit14@localhost server. `ssh -i sshkey.private bandit14@localhost -p 2220` which would log us in as bandit14. 
  - Then we could just cat the contents of `/etc/bandit_pass/bandit14` to get the password. 
  - Then I found a blog post saying to use `scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private . ` which allows us to connect to the remote machine and get the key on our machine locally. Now after the host `bandit.labs.overthewire.org` the file path is then expected. 
  - We can use `:sshkey.private .` as its in the home directory and specify `.` to save it to our current directory. 

### Level 14 -> 15
- password needed: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
  - The password for this level can be retrieved by submitting the password of the current level to port 30000 on localhost.
  - I done this by looking at the telnet utility `man telnet`
  - `telnet localhost 30000` then pasting in the password needed from previous level.
  - It could also be achieved by using `nc` or `netcat` in a similar fashion:
    - `nc localhost 30000`

### Level 15 -> 16
- password needed: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
  - The password for this level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
  - Using `openssl` I was able to do this but I had a few problems with the commands and had to read the manual for openssl then use the `--help` flag to see how to use it. `openssl s_client --help` showed:
    - `Usage: s_client [options] [host:port]` which was different to before with telnet.
    - `openssl s_client localhost:30001` connected me localhost on port 30001 and I was able to submit the password from the previous level to get the next one.
    - Now the official syntax I learned after was to use the `-connect` flag as some builds allow us to give the host:port directly so we should always use:
      - `openssl s_client -connect [host:port]`

### Level 16 -> 17
- **Password needed:** I stored this in an `sshkey17.private` file.
  - **Don't forget:** The file needs the correct permissions: `chmod 600` - read/write only for the owner. Some people said 700 but I read thats only for the directory `~/.ssh`.
  - The way we achieved this was to first find out which ports have a server listening on them using `nmap`. The challenge asked to look for a port on localhost in the range `31000-32000`, so we could do that with:  
    ```bash
    nmap -p 31000-32000 localhost
    ```
  - You could also use the `-sV` option for service discovery, which takes a bit longer but lets you know which ports are running SSL/TLS.
  - Then, using `openssl`, we can connect to the desired port with:  
    ```bash
    openssl s_client -connect localhost:31790
    ```
    (In this instance, `31790/tcp open ssl/unknown` was the port we were looking for.)
  - `OpenSSL s_client` defaults to interactive mode, meaning that every key pressed is interpreted rather than waiting for you to press enter.
  - The keys `k`, `K`, `Q`, and `R` have specific purposes outlined in the manpage for `openssl s_client` under the **CONNECTED COMMANDS** section:
    - `Q` ends the session
    - `R` renegotiates the session
    - `k` sends a key update
    - `k` sends a key update and requests one back
  - Since `s_client` is interactive, if the password happens to start with one of these characters, it will interpret it as a command instead of as part of the data you’re trying to send.
  - The `-quiet` flag prevents this behavior and lets you send your data uninhibited.
- **Source:** [Reddit comment](https://www.reddit.com/r/HowToHack/comments/1g2sivg/comment/mmrz1ua/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

### Level 17 -> 18
- **Password needed:** x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
  - This was to done using the `diff` command to compare two files to see the only line that had been changed between them. If you are familiar with git running `diff $(filename) $(filename2)` would suffice.
  - You can also format it differently using the `-c` flag or make it more complex (or verbose, people seem to love using this word) by adding `diff --side-by-side --suppress-common-lines $(filename) $(filename2)` for a different output.

### Level 18 -> 19
- **Password needed:** cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
  - I approached this slightly differently by using the `scp` command and then to copy the file (readme) where the password was stored. 
  - `scp -P 2220 bandit18@bandit.labs.overthewire.org:readme .` then used cat to view the contents or vim/nvim.
  - I then read that you can can remotely execute commands through ssh as well which is pretty cool to learn.
  - `ssh bandit18@bandit.labs.overthewire.org -p 2220 ls` to make sure the readme is where we were told it was. 
  - Then use `ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme`
