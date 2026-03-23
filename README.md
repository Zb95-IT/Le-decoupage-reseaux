# Exercice de Sous-réseautage — Société fictive à 4 pôles
**Réseau de base** : 172.16.1.0/24
**Objectif** : Découper le réseau en sous-réseaux adaptés aux 4 pôles de l'entreprise

# Présentation de l'entreprise
Pôle	Nombre de machines
Informatique	50
Administratif	20
Technicien	15
Développement	12
Rappels théoriques
Fonctionnement d'un sous-réseau
Un réseau /24 contient 256 adresses (de .0 à .255).
Parmi ces 256 adresses, 254 sont utilisables car :

La première adresse est l'adresse réseau (ex : .0)

La dernière adresse est le broadcast (ex : .255)

Choisir la taille d'un bloc
On choisit toujours la puissance de 2 immédiatement supérieure au nombre de machines + 2 (réseau + broadcast).

Puissance	Taille du bloc	Masque CIDR	Adresses utilisables
2⁶	64	/26	62
2⁵	32	/27	30
2⁴	16	/28	14
On ne peut jamais avoir un bloc de 30 ou 31 : le binaire impose des puissances de 2.

Méthode 1 — Découpage Symétrique
En symétrique, tous les sous-réseaux ont la même taille.
Le pôle le plus grand impose la taille : 50 machines → bloc de 64 → /26.

On avance de 64 en 64 dans le réseau.

# Calcul des sous-réseaux
Pôle	Adresse réseau	Broadcast	Plage d'hôtes	Machines	Places libres gaspillées
Informatique	172.16.1.0/26	172.16.1.63	.1 → .62	50/62	12
Développement	172.16.1.64/26	172.16.1.127	.65 → .126	12/62	50 
Administratif	172.16.1.128/26	172.16.1.191	.129 → .190	20/62	42 ❌
Technicien	172.16.1.192/26	172.16.1.255	.193 → .254	15/62	47 ❌
Total gaspillé : 151 adresses sur 254 → plus de la moitié inutilisée ❌

 Méthode 2 — Découpage Asymétrique 
En asymétrique, chaque sous-réseau a une taille adaptée à son pôle.
**Règle fondamentale : on trie du plus grand au plus pet
