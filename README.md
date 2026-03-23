
# Sous-réseautage IPv4 — Société fictive à 4 pôles

**Réseau de base :** `172.16.1.0/24`

---

## Contexte

L'entreprise est composée de 4 pôles informatiques avec les besoins suivants :

| Pole | Machines requises |
|---|---|
| Informatique | 50 |
| Administratif | 20 |
| Technicien | 15 |
| Developpement | 12 |

---

## Rappels theoriques

### Structure d'une adresse reseau

Dans un bloc d'adresses, deux adresses sont toujours reservees :

- La **premiere** adresse est l'adresse reseau (ex : `.0`)
- La **derniere** adresse est le broadcast (ex : `.255`)

Le nombre d'hotes utilisables est donc : `taille du bloc - 2`

### Choisir la taille d'un bloc

On choisit la puissance de 2 immediatement superieure au nombre de machines + 2.

| Puissance | Taille du bloc | Masque CIDR | Hotes utilisables |
|---|---|---|---|
| 2^6 | 64 | /26 | 62 |
| 2^5 | 32 | /27 | 30 |
| 2^4 | 16 | /28 | 14 |

> Les blocs sont obligatoirement des puissances de 2. Un bloc de 30 ou 31 n'existe pas en binaire.

---

## Methode 1 — Decoupe symetrique

Tous les sous-reseaux ont la meme taille. Le pole le plus grand impose la taille :
50 machines -> bloc de 64 -> `/26`

On avance de **64 en 64** dans le reseau.

| Pole | Adresse reseau | Broadcast | Plage d'hotes | Machines | Gaspillage |
|---|---|---|---|---|---|
| Informatique | 172.16.1.0/26 | 172.16.1.63 | .1 -> .62 | 50 / 62 | 12 |
| Developpement | 172.16.1.64/26 | 172.16.1.127 | .65 -> .126 | 12 / 62 | 50 |
| Administratif | 172.16.1.128/26 | 172.16.1.191 | .129 -> .190 | 20 / 62 | 42 |
| Technicien | 172.16.1.192/26 | 172.16.1.255 | .193 -> .254 | 15 / 62 | 47 |

**Total gaspille : 151 adresses sur 254**

---

## Methode 2 — Decoupe asymetrique

Chaque sous-reseau a une taille adaptee a son pole.
Regle : on trie du plus grand au plus petit avant de commencer.

### Selection des blocs

| Pole | Machines | Besoin reel | Bloc choisi | Masque |
|---|---|---|---|---|
| Informatique | 50 | 52 | 2^6 = 64 | /26 |
| Administratif | 20 | 22 | 2^5 = 32 | /27 |
| Technicien | 15 | 17 | 2^5 = 32 | /27 |
| Developpement | 12 | 14 | 2^4 = 16 | /28 |

### Calcul des sous-reseaux

**Pole Informatique — /26 (bloc de 64)**

