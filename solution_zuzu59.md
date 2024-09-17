zf200619.1200

<!-- TOC titleSize:2 tabSpaces:2 depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 skip:0 title:1 charForUnorderedList:* -->
## Table of Contents
* [Questions générales](#questions-générales)
* [Requêtes de base](#requêtes-de-base)
  * [J'en suis ICI !](#jen-suis-ici-)
* [Filtres de recherche](#filtres-de-recherche)
* [Base de recherche, groupe et unité organisationnelle](#base-de-recherche-groupe-et-unité-organisationnelle)
* [Filtres avancés](#filtres-avancés)
* [Pour aller plus loin](#pour-aller-plus-loin)
* [Requêtes sur Active Directory avec un ticket Kerberos](#requêtes-sur-active-directory-avec-un-ticket-kerberos)
<!-- /TOC -->

# Questions générales

1. Lightweight Directory Access Protocol

    Annuaire pour stocker des informations d'une infrastructure informatique. 
    
    Comme par exemple :
    - Lieux
    - Ordinateurs
    - Personnes
    - Groupes
    - Home directory
    - OU


1. Les annuaires LDAP ont des champs c, o, dc, ou, dn, cn et sn.

  Que signifient-ils ?

  - c = Country 
  - o = Organization
  - dc = Domain Component
  - ou = Organizational unit
  - dn = Distinguished name
  - cn = Common name 
  - sn = Surname


1. Quels sont les sujets des [RFC 4511](https://tools.ietf.org/rfc/index) et suivantes ?

  Tous se trouvent ici:

  https://ldap.com/ldap-related-rfcs/    
  
  
# Requêtes de base

ATTENTION: il faut être en VPN !

1. Avec la commande `ldapsearch`, se connecter sur le `ldapuri`
   '**ldaps://ldap.epfl.ch**' avec une `searchbase` égale à '**c=ch**' et le
   paramètre '**-x**' (Use simple authentication instead of SASL). Que
   constate-t-on ?

   ```
   ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch'
   ```
   
   On liste tout l'annuaire de l'epfl
   

1. Ajouter un filtre `uniqueIdentifier=` égale à votre numéro sciper à la
  commande précédente.
  
  ```
  ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' uniqueIdentifier=106785
  ```
  
  Cela sort tout mon profile qui se trouve dans le LDAP de l'EPFL
  

1. Ajouter le paramètre `-LLL` à la commande précédente. Que fait-il ?

  ```
  ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' uniqueIdentifier=106785 -LLL
  ```
  Je n'ai plus que le résumé !


1. Spécifier les attributs que vous souhaitez obtenir à la fin de votre
   commande, par exemple `personalTitle gecos mail`.

  ```
  ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' uniqueIdentifier=106785 -LLL personalTitle gecos mail
  ```
  

1. Comment modifier cette requête pour rechercher des personnes avec leur nom de
   famille exact ?
   
   ```
   ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' cn=zufferey -LLL personalTitle gecos mail
   ```
   Cela sort tous les zufferey de l'EPFL


1. Comment modifier cette requête pour rechercher une personne avec son email
   exact ?

   ```
   ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' mail=christian.zufferey@epfl.ch -LLL personalTitle gecos mail
   ```



## J'en suis ICI !





    
# Filtres de recherche
1. Quel est le [caractère
  "wildcard"](https://fr.wikipedia.org/wiki/M%C3%A9tacaract%C3%A8re) utilisé 
  dans les filtres LDAP ?
1. Donner 2 requêtes basées sur celles de l'exercice précédent utilisant le 
  caractère wildcard.

# Base de recherche, groupe et unité organisationnelle
1. Trouver tous les emails des personnes dans l'unité IDEV-FSD.
1. Trouver tous les usernames des personnes dans le groupe 
  [epfl-dojo](https://groups.epfl.ch/cgi-bin/groups/viewgroup?groupid=S13602).
1. Expliquer les différents types d'organisation que donne la requête 
  `ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' -LLL 'objectclass=organization' dn`

# Filtres avancés
1. Trouver toutes les personnes ayant un compte "guest" présentent dans le 
  groupe [epfl-dojo](https://groups.epfl.ch/cgi-bin/groups/viewgroup?groupid=S13602).
1. Trouver les personnes de l'unité IDEV-FSD qui ne sont pas dans le 
  groupe [epfl-dojo](https://groups.epfl.ch/cgi-bin/groups/viewgroup?groupid=S13602).

# Pour aller plus loin
1. Compter le nombre de personnes avant le nom de famille "Dupont" dans l'annuaire.
1. Extraire la liste d'email du groupe "epfl-dojo", vérifier qu'il n'y a pas de 
  doublon, la trier par ordre alphabétique et la sauver dans un fichier.
1. Compter le nombre d'unité à l'EPFL.
1. Compter le nombre de Professeurs dans l'école.
1. Trouver la personne avec le numéro sciper le plus élevé.

# Requêtes sur Active Directory avec un ticket Kerberos
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



