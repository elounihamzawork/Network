# Network

**IPTABLES (outil d'administration pour le filtrage de paquets IPv4 et le NAT)**

- Nous permet d'installer un pare-feu(firewall) qui va permettre de controller les connexions entrantes et sortantes
  aves nos serveurs. On peut fermer tous les connexions et ensuite on va ouvrir que les ports dont on a besoin.
- Nous permets de modifier les paquets
- Pour l'installer:  ``sudo apt-get install iptables``
- Pour voir la liste des règles que vous avez: ``sudo iptables -L`` => on a trois groupes (INPUT: pour les paquets rentrants,
  FORWARD: pour les paquets que vous allez rediriger, OUTPUT: pour les paquets sortants)
- Par défaut on une politique d'acceptation(policy ACCEPT)


### Commandes

- iptables -F: Effacer tous les règles une par une 
- iptables -P [policy name] DROP: bloquer la politique selectionné(INPUT/OUTPUT/FORWARD), ca va bloquer toute connexion vers ce politique

## Tables

**Table NAT**
- Cette table s'occupe de la translation de ports et d'ip. On deux localistaions:(avant le parefeu `PREROUTING` et apres le parefeu ``POSTROUTING``)
on peut faire des choses en postrouting et en prerouting.
- On 3 types de target dans cette table : `DNAT` l'ip de distination, `SNAT` l'ip source, `MASQURADE` simule une gateway

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


