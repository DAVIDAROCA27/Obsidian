We can use a very powerful tool called [Hashcat](https://hashcat.net/hashcat/) to combine lists of potential names and labels with specific mutation rules to create custom wordlists. To become more familiar with Hashcat and discover the full potential of this tool, we recommend the module [Cracking Passwords with Hashcat](https://academy.hackthebox.com/course/preview/cracking-passwords-with-hashcat). Hashcat uses a specific syntax for defining characters and words and how they can be modified. The complete list of this syntax can be found in the official [documentation](https://hashcat.net/wiki/doku.php?id=rule_based_attack) of Hashcat. However, the ones listed below are enough for us to understand how Hashcat mutates words.

|**Function**|**Description**|
|---|---|
|`:`|Do nothing.|
|`l`|Lowercase all letters.|
|`u`|Uppercase all letters.|
|`c`|Capitalize the first letter and lowercase others.|
|`sXY`|Replace all instances of X with Y.|
|`$!`|Add the exclamation character at the end.|

Each rule is written on a new line which determines how the word should be mutated. If we write the functions shown above into a file and consider the aspects mentioned, this file can then look like this:

#### Hashcat Rule File

  Password Mutations

```
pepedavidphone@htb[/htb]$ cat custom.rule
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
Hashcat will apply the rules of `custom.rule` for each word in `password.list` and store the mutated version in our `mut_password.list` accordingly. Thus, one word will result in fifteen mutated words in this case.

#### Generating Rule-based Wordlist

```
pepedavidphone@htb[/htb]$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
pepedavidphone@htb[/htb]$ cat mut_password.list
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

`Hashcat` and `John` come with pre-built rule lists that we can use for our password generating and cracking purposes. One of the most used rules is `best64.rule`, which can often lead to good results. It is important to note that password cracking and the creation of custom wordlists is a guessing game in most cases. We can narrow this down and perform more targeted guessing if we have information about the password policy and take into account the company name, geographical region, industry, and other topics/words that users may select from to create their passwords. Exceptions are, of course, cases where passwords are leaked and found.

#### Hashcat Existing Rules

  Password Mutations

```shell-session
pepedavidphone@htb[/htb]$ ls /usr/share/hashcat/rules/

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

We can now use another tool called [CeWL](https://github.com/digininja/CeWL) to scan potential words from the company's website and save them in a separate list. We can then combine this list with the desired rules and create a customized password list that has a higher probability of guessing a correct password. We specify some parameters, like the depth to spider (`-d`), the minimum length of the word (`-m`), the storage of the found words in lowercase (`--lowercase`), as well as the file where we want to store the results (`-w`).

#### Generating Wordlists Using CeWL

  Password Mutations

```shell-session
pepedavidphone@htb[/htb]$ cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
pepedavidphone@htb[/htb]$ wc -l inlane.wordlist

326
```