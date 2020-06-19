#Questions générales

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
  
  
  ### Requêtes de base
  
  ATTENTION: il faut être en VPN !
  
  1. Avec la commande `ldapsearch`, se connecter sur le `ldapuri`
     '**ldaps://ldap.epfl.ch**' avec une `searchbase` égale à '**c=ch**' et le
     paramètre '**-x**' (Use simple authentication instead of SASL). Que
     constate-t-on ?

     ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch'
  
     On liste tout l'annuaire de l'epfl
     
  1. Ajouter un filtre `uniqueIdentifier=` égale à votre numéro sciper à la
    commande précédente.
    
    ldapsearch -x -H ldaps://ldap.epfl.ch -b 'c=ch' uniqueIdentifier=106785
    
    Cela sort tout mon profile qui se trouve dans le LDAP de l'EPFL
    
    
    
    
    
  