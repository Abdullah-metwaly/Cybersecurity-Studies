# Password List

> Hashcat enables us to mutate the passwords we have in mind to make the brute forcing more efficient we can use results from new mutated passwords with [[Network Services]] module to attack the service with #hydra 

|**Function**|**Description**|
|---|---|
|`:`|Do nothing.|
|`l`|Lowercase all letters.|
|`u`|Uppercase all letters.|
|`c`|Capitalize the first letter and lowercase others.|
|`sXY`|Replace all instances of X with Y.|
|`$!`|Add the exclamation character at the end.| 

for the word `password` with below applied rules we can get several possibilities 

### Hashcat Rule File

``` 
$ cat custom.rule
:
c
so0
c so0
sa@
c sa@
c sa@ so0
$!
$! c
$! so0
$! sa@
$! c so0
$! c sa@
$! so0 sa@
$! c so0 sa@
```

	Here are the results from above rules after using hashcat
### Generating Rule-based Wordlist

```
$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```

```
$ cat mut_password.list
password
Password
passw0rd
Passw0rd
p@ssword
P@ssword
P@ssw0rd
password!
Password!
passw0rd!
p@ssword!
Passw0rd!
P@ssword!
p@ssw0rd!
P@ssw0rd!
```

### Hashcat Existing Rules


```
$ ls /usr/share/hashcat/rules/

best64.rule                  specific.rule
combinator.rule              T0XlC-insert_00-99_1950-2050_toprules_0_F.rule
d3ad0ne.rule                 T0XlC-insert_space_and_special_0_F.rule
dive.rule                    T0XlC-insert_top_100_passwords_1_G.rule
generated2.rule              T0XlC.rule
generated.rule               T0XlCv1.rule
hybrid                       toggles1.rule
Incisive-leetspeak.rule      toggles2.rule
InsidePro-HashManager.rule   toggles3.rule
InsidePro-PasswordsPro.rule  toggles4.rule
leetspeak.rule               toggles5.rule
oscommerce.rule              unix-ninja-leetspeak.rule
rockyou-30000.rule
```


#hashcat #password_mutations #network_service  

### Generating Wordlists Using CeWL

```
$ cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
$ wc -l inlane.wordlist
326
```

#cewl #password_mutations #network_service 