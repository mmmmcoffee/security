# PowerShell

PowerShell is Windows new shell. It comes by default from Windows 7. But can be downloaded and installed in earlier versions.

* PowerShell provides access to almost everything an attacker might want.
* It is based on the .NET framework.
* It is basically bash for windows
* The commands are case-insensitive

## Basics

So a command in PowerShell is called **cmdlet**. The cmdlets are created using a verb and a noun. Like `Get-Command`, Get is a  verb and  Command is a noun. Other verbs can be: remove, set, disable, install, etc.



To get help on how to use a **cmdlet** while in PowerShell, the man-page, you do:

```
Get-Help    <cmdlet    name    |    topic    name>
```

Example

```
get-help echo
get-help get-command
```

**Powershell Version and Build**

```
$PSVersionTable
```

### Fundamentals

With get-member you can list all the properties and methods of the object that the command returns.

```
Get-Member
For example:
Get-Command | Get-Member
Get-Process | Get-Member
```



Select-XXX

```
Select-object
```

Print to the console:
Write-Host "hello"

#### Variables

```
$testVar = "blabla"
$testVar.GetType() #Displays variable type

Arrays:
$my_array = @() #empty array
$my_array = @("abc","def","ghi") # 3 values
$my_array+= "jkl" # add to the array (expensive operation)

PS C:> $my_array[1]
def

PS C:> $my_array[1..3]
def
ghi
jkl
```

####Loops
for ($i = 0; $i -le 5; $i++){
    write-host "In loop number $i"
   }


$my_array = @("abc","def","ghi") 
foreach($i in $my_array){
    write-host "array value is $i"
 }


**Wget / Download a file**

```
Invoke-WebRequest <uri>
wget <uri>
```

**Grep**

```
Select string can be used like grep
get-command | select-string blabla
```

**General commands that can be used on objects**

```
measure-object -words
get-content fil.txt | measure-object words
```

### Working with filesystem

**List all files in current directory**

```
get-childitem
gci

List hidden files too
gci -Force

List all files recurisvely
gci -rec

Count the files
(get-childitem).count
List all files but exclude some folders
gci -exclude AppData | gci -rec -force
```

### Working with files

```
Read a file
Get-Content
    gc
    cat
Count lines of file
(get-content .\file).count
Select specific line in a file (remember that it starts from 0)
(gc .\file.txt)[10]
gc .\file.txt | Select -index 10


Create/Editing files:
New-Item -path "myfile.txt" -ItemType "file"
Set-Content "myfile.txt" -Value "This will overwrite any contents"
Add-Content "myfile.txt" -Value "This appends to the file"
```

###Working with 'non cmdlet functions'
```
$n = netstat -a -o
$n = $n[4..$n.length] #trim the beginning lines returned from netstat
$o = @() #array of empty objects
foreach ($line in $n){
       $linesplit = $($line -split "\s+");
       $o+= New-Object PSObject -Property @{Proto=$linesplit[1]; Address= $linesplit[2], PID=$linesplit[5]};
    }

$n | Select PID #Returns Empty
$o | Select PID $Piping and filtering works with objects!
```

### Services

```
List services
get-service
```

### Getting Hashes

```
Get File Hashes:
Get-FileHsah -Algorithm [MACTripleDES|MD5|RIPEMD160|SHA1|SHA256|SHA384|SHA512]


Getting hashes of files recursively of a particular directory:
Get-ChildItem "C:\Users\mystuff" -Recurse -Attributes !Directory | Get-FileHash -Algorithm MD5 | Select Hash, Path | Format-Table -Autosize

### Network related stuff

Domain information

```
Get-ADDomain
Get-AdDomainController
Get-AdComputer
To see a list of all properties do this
get-adcomputer ComputerName -prop *

Get AD Users
Get-ADUser -f {Name -eq 'Karl, Martinez'} -properties *

Get all AD Groups
Get-ADGroup -filter *



Resolve DNS
Resolve-DNSname 10.10.10.10

```



