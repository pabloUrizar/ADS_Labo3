# ADS Lab 03 - Pipeline

Authors: Vincent Peer, Pablo Urizar

Date: March 9, 2023

### 1. Run the following commands and tell where stdout and stderr are redirected to.  
> a. ./out > file  
​
stdout is redirected to the file. stderr still points to the command prompt.  
> b. ./out 2> file   
​
stdout still points to the command prompt. stderr is redirected to the file.   
> c. ./out > file 2>&1   
 
stdout is redirected to the file. stderr is redirected to stdout, thus to the file, so both to the file.  
> d. ./out 2>&1 > file  
 
First, stderr is redirected to stdout, thus to the command prompt. Then stdout is redirected to the file but stderr is still pointing to the command prompt.  
> e. ./out &> file  
 
stdout and stderr are both redirected to the file.   
​ 

### 2. What do the following commands do?
> a. cat /usr/share/doc/nano/README | grep -i edit

It will display the lines that contain the word "edit" in the provided README file.

> b. ./out 2>&1 | grep –i eeeee

stderr will be redirected to stdout so both will be displayed in the command prompt. From the obtained result, we will keep only the lines that contain "eeeee".

> c. ./out 2>&1 >/dev/null | grep –i eeeee

stderr will be redirected to stdout but it does not change anything because stderr was alread directed to the command prompt output. Then, only the lines that contain "eeeee" will be displayed.

### 3. Write commands to perform the following tasks:

## Task 2: Log analysis



1. How many log entries are in the file?
```  
> La commande wc avec l'option -l permet de compter le nombre de ligne d'un fichier : wc -l ads_website.log
> 2781 ads_website.log
```

2. How many accesses were successful (server sends back a status of 200) and how
many had an error of "Not Found" (status 404)?  
```
> cut -f 10 ads_website.log | grep 404 | wc -l
```


3. What are the URIs that generated a "Not Found" response? Be careful in
specifying the correct search criteria: avoid selecting lines that happen to
have the character sequence 404 in the URI.  
> cut -f 9-10 ads_website.log | grep '404$' | sort | uniq | cut -d ' ' -f 2
/heigvd-ads?cors
/heigvd-ads?lifecycle
/heigvd-ads?policy
/heigvd-ads?website  

Prend uniquement les champs 9 (type de requête, son URI siuvi par HTTP/1.1) et 10 (status de réponse)  
Retient uniquement les résultats qui se terminent par 404
Trie puis rend unique chaque résultat
Enfin, sépare les éléments ayant un espace en champ différent avant d'afficher uniquement le champ 2 qui est L'URI.


4. How many different days are there in the log file on which requests were made?  
```   
$ cat ads_website.log | cut -f 3 | cut -d ':' -f 1 | sort | uniq | wc -l  
21  


Prend le champ avec la date (ex : [19/Sep/2020:16:30:00 +0000])
Sépare avec le délimiteur ':', prend le 1er champ avec jour/mois/année
Trie et rend unique chaque ligne
Compte le nombre de ligne 
```



5. How many accesses were there on 4th March 2021?  
```
$ cut -f3  ads_website.log | grep '04/Mar/2021' | wc -l   
423
```
6. Which are the three days with the most accesses? Hint: Create first a pipeline
that produces a list of dates preceded by the count of log entries on that
date.
```
$ cut -f3 ads_website.log | cut -d ':' -f1  | uniq -c | sort -r | head -n 3 | cut -c 10- 
898 [13/Mar/2021
580 [06/Mar/2021
423 [04/Mar/2021
Prend le 3ème champ
Sépare avec le délimiteur ':' puis ne prend que le premier champ
Rend unique en indiquant en préfixe de chaque ligne son nombre d'occurence
Trie de façon décroissante
Retient uniquement les 3 premiers
Tronque le début de ligne pour ne laisser que la date
```

7. Which is the user agent string with the most accesses?
```
$ cut -f 17 ads_website.log | sort | uniq -c | sort -r | head -n 1
423 "Mozilla/5.0 (Windows NT 6.3; WOW64; rv:27.0) Gecko/20100101 Firefox/27.0"
Prend le champ 17
Trie par nom d'agent
Rend unique et indique en préfixe de chaque ligne son nombre d'occurence
Trie de façon décroissante
Affiche la première ligne
```
8. If a web site is very popular and accessed by many people the user agent
strings appearing in the server's log can be used to estimate the relative
market share of the users' computers and operating systems. How many accesses
were done from browsers that declare that they are running on Windows, Linux
and Mac OS X (use three commands)?
```
Mac OS X :
$ cut -f 17 ads_website.log | grep 'Mac OS X' | wc -l
693

Windows :
$ cut -f17 ads_website.log | grep "Windows" | wc -l
1751

Linux :
$ cut -f17 ads_website.log | grep "Linux" | wc -l
180

Prend le champ 17
Recherche les lignes qui contienne 'Mac OS X' / 'Windows' / Linux
Compte le nombre de ligne affichée
```
9. Read the documentation for the tee command. Repeat the analysis of the
previous question for browsers running on Windows and insert tee into the
pipeline such that the user agent strings (including repeats) are written to a
file for further analysis (the filename should be useragents.txt ).
```
$ cut -f17 ads_website.log | grep "Windows" | tee useragents.txt
Prend le champ 17
Recherche les lignes qui contienne 'Windows'
Affiche le résultat dans la sortie standard ainsi que dans le fichier useragents.txt
```


## Task 3: Conversion to CSV