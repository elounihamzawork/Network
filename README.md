# Network

**IPTABLES (outil d'administration pour le filtrage de paquets IPv4 et le NAT)**

- Nous permet d'installer un pare-feu(firewall) qui va permettre de controller les connexions entrantes et sortantes
  aves nos serveurs. On peut fermer tous les connexions et ensuite on va ouvrir que les ports dont on a besoin.
- Nous permets de modifier les paquets
- Pour l'installer:  ``sudo apt-get install iptables``
- Pour voir la liste des règles que vous avez: ``sudo iptables -L`` => on a trois groupes (INPUT: pour les paquets rentrants,
  FORWARD: pour les paquets que vous allez rediriger, OUTPUT: pour les paquets sortants)
- Par défaut on une politique d'acceptation(policy ACCEPT)
- IPTABLES prend les regles dans l'ordre (L'ordre des regles est important)
- Le principe pour sécuriser un réseau : on refuse tous entrants/sortants puis on autorise au fur et à mesure des besoins


### Commandes

- iptables -F: Effacer tous les règles une par une 
- iptables -P [policy name] DROP: bloquer la politique selectionné(INPUT/OUTPUT/FORWARD), ca va bloquer toute connexion vers ce politique
- iptables -A: ajouter de règle à une chaine(ajouter la règle à la fin de l'ensemble des regles de cette chaine)
- iptables -D: suppression de règle (-D INPUT 1 => le 1 c'est le numéro de la règle dans la chaine INPUT)
- iptables -R: remplacer la règle
- iptables -I: insertion du règle(sans chiffre au debut de la chaine)
- iptables -N: création de chaine
- iptables -X: drop de chaine
- iptables -P: définition de la policy d'une chaine

## Tables

**Table NAT**
- Cette table s'occupe de la translation de ports et d'ip. On a deux localistaions:(avant le parefeu `PREROUTING` et apres le parefeu ``POSTROUTING``)
  on peut faire des choses en postrouting et en prerouting.
- On a 3 types de target dans cette table : `DNAT` l'ip de distination, `SNAT` l'ip source, `MASQURADE` simule une gateway

**Table Filter**

- Elle est composer de 3 chaines: ``INPUT``, ``OUTPUT``, ``FORWARD``
- On 4 targets: ``DROP``(refus brut des paquets sans retours), ``ACCEPT``(accepte les paquets), ``REJECT``(rejet avec retour à l'expediteur)
  , ``DENY``, ``LOG``(Tracer , logger les paquets)

**Table Mangle (Modification des paquets)**

Elle a 5 targets:
- TOS : Type of service
- TTL : Durée de vie(time to live)
- MARK: Marquer les paquets(taggage)
- SECMARK: Marquage de sécurité(pour outils de sécurité type SE linux)
- CONNSECMARK: copie d'un cas de sécurité


### Mise en pratique

**Le Ping ?**

Quand on a deux machines, une machine peut aller pinger autre machine pour savoir si elle est UP.
``Ping =  Protocole ICMP``

Empecher les ping en entrée ?
Supposons on travaille sur la machine 172.17.0.2 et une autre machine 172.17.0.1 veut pinger sur notre machine, comment on va 
empecher le ping ?

``iptables -I INPUT -p icmp --icmp-type 8 -j DROP``

**Supprimer une régle précise**
```
iptables -L --line-numbers
iptables -D [TYPE_CHAINE] "num_ligne"
```

**Refuse toute connexion sur le port 8000**
``iptables -I INPUT -p tcp --dport 8000 -j DROP``

**Accepter le traffic qui vient d'une machine (conf ssh)**
``iptables -I INPUT -s(source) 127.17.0.1 -p tcp --dport 8000 -j ACCEPT``

**supprimer toutes les regles**
``iptables -t filter -F ``

**pour laisser l'access à une machine**
``iptables -A INPUT state --state RELATED,ESTABLISHED -j ACCEPT`` (-A: ajout de la règle à la fin)
``iptables -A OUTPUT state --state RELATED,ESTABLISHED -j ACCEPT``

**Accepter les ping**
``iptables -t filter -A INPUT -p icmp -j ACCEPT``
``iptables -t filter -A OUTPUT -p icmp -j ACCEPT``




