
# Reset a broken TTY 
http://superuser.com/questions/640338/how-to-reset-a-broken-tty


# Find out what processes are making network connections
http://shallowsky.com/blog/linux/monitor-net-connections.html


# To find repeating entries in a file
cat file-name | sort | uniq -c | sort -nr | awk '{if ($1>1) print $0}'


# To print first n lines of all files having some extension, say, "ano" in a directory
find . -type f -name "*.ano" -exec awk -F'\t' '{if (($1==1)&&(k==0)) {k++; print $0 }}' \{\} \;


# XML: to split a large xml like document into smaller ones based on a root tag, say "DOC"
csplit -ksf part. /path/to/file.xml /\<DOC\>/ "{3645}" 2>/dev/null


# XML: append a tag (say, "trectext") at the end 
find . -type f -exec sed -i '$a </trectext>' {} \;


# XML: pre-Append a tag (say, "trectext")
find . -type f -exec sed -i '1i <trectext>' {} \;


# Longest word in a file
cat file-name | sed 's/ /\n/g' | sort | uniq | awk '{print length, $0}' | sort -nr | head


# Symlink all files in a directory
cp -rs  cp -rs /path/to/directory .


# To find the last log files that were modified
find $1 -type f -exec stat --format '%Y :%y %n' {} \; | sort -nr | cut -d: -f2- | head



# diff two dirs but only files that match a particular name pattern
diff -qr /path/to/directory1 /path/to/directory2 -x "*_DIFF_*"


# To compare two files (common / differing lines) - http://www.cyberciti.biz/faq/command-to-display-lines-common-in-files/
grep -v -f file-name-1 file-name-2 | wc -l


# To generate n (say 50) random numbers in a range (say, 1 to 4202)
shuf -i 1-4202 -n 50


# Pick a line number from a file, method 1
sed '52!d'


# Pick a line number from a file - method 2, efficient on large files
sed '52q;d'


# Read a line (say corresponding to line number 22430) from a file to another file
sed -n 22430,22631p file-name > new-file-name


# Recursively grep for files containing some string (say, 'pattern') and have some extension (say, xml)
find . -name '*.xml' -exec grep -l 'pattern' {} \; -print


# Setting permissions correctly
http://www.yolinux.com/TUTORIALS/LinuxTutorialManagingGroups.html


# XML: Move one tag into another. Example ABSTRACT tag is being moved inside the TEXT tag. Replace <TEXT> with empty
grep -rl "<TEXT>" . | xargs sed -i "s@<TEXT>@@g"


# XML Replace <ABSTRACT> with <TEXT>\n<ABSTRACT>
grep -rl "<ABSTRACT>" . | xargs sed -i "s@<ABSTRACT>@<TEXT>\\n<ABSTRACT>@g"


# Renumber a file in AWK
cat file-name | awk -F'\t' '{printf "%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\n", FNR+3010000-1, $1,$2,$3,$4,$5,$6,$7,$8}' | head


# Print only the pid from ps matching a process name, say ruby
ps -ef | grep ruby | awk -F" " '{print $2}'


# Transform column to rows
awk '{for (i=1;i<=NF;i++) print $i}' file-name


# To extract text between two line numbers
sed -n '1021472,1023205p' filename


# Move folders older than certain days to some other folder, say "old_logs"
find . -type d -ctime +4 -exec mv {} ../old_logs \;
 	

# sum-integers-one-per-line:
awk '{s+=$1} END {print s}'


# Group by and average:
awk 'BEGIN{FS=":"; print "item count total avg"} NR!=1 {a[$1]++;b[$1]=b[$1]+$2}END{for (i in a) printf("%s %10.0f %10.0f %10.2f\n", i, a[i], b[i], b[i]/a[i])} ' file-name
 

# 95th percentile
awk 'BEGIN{i=0} {s[i]=$1; i++;} END{print s[int(NR*0.95-0.5)]}'


# Find orphan processes
ps -elf | awk '{if ($5 == 1){print $4" "$5" "$15}}' | grep indrid | awk '{print $1}'


# Kill all processes of a user
pkill -9 -u `id -u username`



# If a file has a single very long line, if you just want to grep around the regex containing string that is being searched, use
# grep -o -b "<search-string>" <large-file> and then
grep "<search-string>" <large-file> | awk ' { print substr($0,<begin_index>,<begin_index>,<begin_index>+<width>) }'


# VPN tunnel 
ssh -L 5901:remote-target-machine:5901 user@gateway-machine


# Java - find a class in all jars:
find . -type f -name '*.jar' -print0 |  xargs -0 -I '{}' sh -c 'jar tf {} | grep com/mypackage/Hello.class &&  echo {}'


# Truncate a large file
truncate -s 0 file-name


# Shell script - check if file exists in 
if test -e "$file_name";then


# Find files matching a regex
find . -regex '.*myfile[0-9][0-9]?'


# Find average file size
ls -l | gawk '{sum += $5; n++;} END {print sum/n;}'


# Select 10 files in random from a directory and copy to some other directory:
find . -type f | shuf -n 10 | xargs -I {} cp {} /path/to/directory	


#  SED: replace multiple files (say all files whose name contains baz): http://unix.stackexchange.com/questions/112023/how-can-i-replace-a-string-in-a-files
sed -i -- 's/foo/bar/g' *baz*   


# Find & Kill orphan process
ps -elf | head -1; ps -elf | awk '{if ($5 == 1 && $3 != "root") {print $0}}' | head


# Recursive find and replace of a string in a directory: http://stackoverflow.com/questions/1583219/awk-sed-how-to-do-a-recursive-find-replace-of-a-string	
find /home/www \( -type d -name .git -prune \) -o -type f -print0 | xargs -0 sed -i 's/subdomainA\.example\.com/subdomainB.example.com/g'


# Get total files size from a file, say filelist.txt, containing a file list
cat filelist.txt | xargs ls -l | awk '{x+=$5} END {print "total bytes: " x}' 


# awk find and replace 
gensub(r, s, h [, t])


# Post a file to a rest service as a form payload param
curl  -X POST -i --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/xml' --data-binary 'text=@/path/to/file' 'http://path/'


# List all SSH tunnels
sudo lsof -i -n | egrep '\<sshd\>'


# List of all VNC port forwards
sudo lsof -i -n | egrep '\<sshd\>' | grep -v root | grep ":59"  | awk '{print $3, $(NF-1)}'


# Print directory structure as a tree
ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\// /g' -e 's/^/ /'


# grep for items in two files
cat file1 | grep -fxF file2

