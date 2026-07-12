# Challenge Lab: Bash Shell Scripting Exercise

## Assignment

Write a Bash script based on the following requirements:
- Creates 25 empty (0 KB) files. (Hint: Use the **touch** command.)
- The file names should be **<yourName><number>, <yourName><number+1>, <yourName><number+2>**, and so on.
- Design the script so that each time you run it, it creates the next batch of 25 files with increasing numbers starting with the last or maximum number that already exists.
- Do not hard code these numbers. You need to generate them by using automation.
- Test the script. Display a long list of the directory and its contents to validate that the script created the expected files.

## My solutions

1. I created a new file called `bash_script.sh`.
```bash
touch bash_scripting.sh
```

2. Then I inserted the code using the `vi` editor
```bash
#!/bin/bash

prefix="Cikizwa"

max=$(ls ${prefix}* 2>/dev/null | sed 's/[^0-9]//g' | sort -n | tail -1)
max=${max:-0}
echo "Previous number: $max"

for i in {1..25}
do
#echo ${prefix}$(($i+$max))
touch ${prefix}$(($i+$max))
done
```

3. I made the file executable and run it.
```bash
chmod 744 bash_scripting.sh
./bash_scripting.sh
```

4. After the first run, here it is displayed the content of my directory.
```bash
[ec2-user@ip-10-0-10-227 ~]$ ./bash_script.sh 
Previous number: 0
[ec2-user@ip-10-0-10-227 ~]$ ll
total 4
-rwxr--r--  1 ec2-user ec2-user  230 Jul 12 22:10 bash_script.sh
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa1
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa10
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa11
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa12
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa13
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa14
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa15
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa16
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa17
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa18
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa19
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa2
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa20
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa21
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa22
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa23
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa24
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa25
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa3
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa4
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa5
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa6
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa7
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa8
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa9
drwxr-xr-x 11 ec2-user Personnel 184 Jul 12 22:01 companyA
```
5. And the content after the second run.
```bash
[ec2-user@ip-10-0-10-227 ~]$ ./bash_script.sh 
Previous number: 25
[ec2-user@ip-10-0-10-227 ~]$ ll
total 4
-rwxr--r--  1 ec2-user ec2-user  230 Jul 12 22:10 bash_script.sh
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa1
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa10
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa11
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa12
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa13
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa14
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa15
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa16
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa17
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa18
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa19
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa2
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa20
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa21
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa22
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa23
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa24
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa25
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa26
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa27
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa28
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa29
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa3
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa30
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa31
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa32
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa33
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa34
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa35
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa36
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa37
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa38
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa39
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa4
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa40
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa41
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa42
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa43
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa44
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa45
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa46
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa47
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa48
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa49
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa5
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:23 Cikizwa50
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa6
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa7
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa8
-rw-rw-r--  1 ec2-user ec2-user    0 Jul 12 22:10 Cikizwa9
drwxr-xr-x 11 ec2-user Personnel 184 Jul 12 22:01 companyA
```
