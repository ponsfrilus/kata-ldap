# Kata LDAP
Kata d'exploration d'un annuaire LDAP (ldapsearch).

> **À propos**
>
> ⓘ Ceci est la donnée d'un [kata], un _exercice de programmation_ qui se
> déroule généralement dans le cadre d'un [coding dojo]. Il est proposé aux
> membres du dojo de l'[EPFL] et fait partie d'une collection de différents
> katas identifiés par le tag [epfl-dojo-kata] sur GitHub. Vous êtes plus que
> bienvenu d'essayer de le réaliser dans le langage de programmation de votre
> choix. Pour cela, un bon point de départ pour cela est de [forker] ce
> repository. Le «[stargazer]» en lui ajoutant une **☆** nous fait également
> très plaisir. Finalement, n'hésitez pas à l'améliorer en faisant des
> [pull requests]. Bonne lecture !

[kata]: https://fr.wikipedia.org/wiki/Coding_dojo#Kata
[coding dojo]: https://fr.wikipedia.org/wiki/Coding_dojo
[EPFL]: https://www,epfl.ch
[epfl-dojo-kata]: https://github.com/topics/epfl-dojo-kata
[forker]: https://docs.github.com/en/get-started/quickstart/fork-a-repo#forking-a-repository
[stargazer]: https://docs.github.com/en/get-started/exploring-projects-on-github/saving-repositories-with-stars
[pull requests]: https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request#creating-the-pull-request


_Note_: les exercices qui suivent sont basés sur le LDAP de l'EPFL. Par
conséquent, ils est possible que certains champs ne soient pas présents dans
d'autres annuaires. Une adaptation des exercices ci-dessous en fonction du
schéma de votre annuaire est probablement nécessaire, bien que les principes de 
base restent les mêmes.


## But

Cet exercice permet de travailler les requêtes sur un annuaire LDAP.


## Comment procéder

1. [Forker](https://github.com/ponsfrilus/kata-ldap/#fork-destination-box) le
repo et créer une branche portant le nom de votre username (`git checkout -b
username` par exemple `git checkout -b ponsfrilus`, depuis votre fork). 

1. Ajouter un fichier `solution_username.md` (par exemple
`solution_ponsfrilus.md`) dans  lequel vous indiquer les réponses aux différents
questions. 

1. Ajouter votre nom d'utilisateur au fichier [CONTRIBUTORS.md](./CONTRIBUTORS.md).

1. Faites ensuite une "pull request" pour ajouter vos solutions à ce repo.


## Données

### Question générales

1. Que signifie LDAP ? ([wikipedia](https://fr.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol))
1. Les annuaires LDAP ont des champs `c`, `o`, `dc`, `ou`, `dn`, `cn` et `sn`.  
   Que signifient-ils ?
1. Quels sont les sujets des [RFC 4511](https://tools.ietf.org/rfc/index) et suivantes ?

### Requêtes de base

1. Avec la commande `ldapsearch`, se connecter sur le `ldapuri`
   '**ldaps://ldap.epfl.ch**' avec une `searchbase` égale à '**c=ch**' et le
   paramètre '**-x**' (Use simple authentication instead of SASL). Que
   constate-t-on ?
1. Ajouter un filtre `uniqueIdentifier=` égale à votre numéro sciper à la
   commande précédente.
1. Ajouter le paramètre `-LLL` à la commande précédente. Que fait-il ?
1. Spécifier les attributs que vous souhaitez obtenir à la fin de votre
   commande, par exemple `personalTitle gecos mail`.
1. Comment modifier cette requête pour rechercher des personnes avec leur nom de
   famille exact ?
1. Comment modifier cette requête pour rechercher une personne avec son email
   exact ?


### Filtres de recherche

1. Quel est le [caractère
   "wildcard"](https://fr.wikipedia.org/wiki/M%C3%A9tacaract%C3%A8re) utilisé 
   dans les filtres LDAP ?
1. Donner 2 requêtes basées sur celles de l'exercice précédent utilisant le 
   caractère wildcard.

### Base de recherche, groupe et unité organisationnelle

1. Trouver tous les emails des personnes dans l'unité IDEV-FSD.
1. Trouver tous les usernames des personnes dans le groupe 
   [epfl-dojo](https://groups.epfl.ch/cgi-bin/groups/viewgroup?groupid=S13602).
1. Expliquer les différents types d'organisation que donne la requête 
   `ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' -LLL 'objectclass=organization' dn`

### Filtres avancés

1. Trouver toutes les personnes ayant un compte "guest" présentent dans le 
   groupe [epfl-dojo](https://groups.epfl.ch/cgi-bin/groups/viewgroup?groupid=S13602).
1. Trouver les personnes de l'unité IDEV-FSD qui ne sont pas dans le 
   groupe [epfl-dojo](https://groups.epfl.ch/cgi-bin/groups/viewgroup?groupid=S13602).

### Pour aller plus loin

1. Compter le nombre de personnes avant le nom de famille "Dupont" dans l'annuaire.
1. Extraire la liste d'email du groupe "epfl-dojo", vérifier qu'il n'y a pas de 
   doublon, la trier par ordre alphabétique et la sauver dans un fichier.
1. Compter le nombre d'unité à l'EPFL.
1. Compter le nombre de Professeurs dans l'école.
1. Trouver la personne avec le numéro sciper le plus élevé.

### Requêtes sur Active Directory avec un ticket Kerberos

1. Les paquets `krb5-usr`, `libsasl2-2`, `libsasl2-modules-gssapi-mit`
2. Le royaume (REALM) Kerberos de l'EPFL est "`INTRANET.EPFL.CH`"
.) Cela devrait configurer le   
   `default_realm = INTRANET.EPFL.CH`
   de la séction `[libdefaults]` du fichier de configuration `/etc/krb5.conf`
.) Pour éviter l'erreur "Server not found in Kerberos database" de GSSAPI, spécifier les adresses des serveurs intranet de l'école dans `/etc/hosts`, e.g.
  `128.178.15.229 ad3.intranet.epfl.ch ad3`
5. Optenir un ticket Kerberos avec la commande `kinit username`
6. Tester une requête avec:  
   `ldapsearch -O maxssf=0 -Y GSSAPI -H ldap://ad3.epfl.ch -LLL -b "dc=intranet,dc=epfl,dc=ch" '(uid=username)'`
