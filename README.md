# algo-tn-compiler

Un transpileur (compilateur source-à-source) qui traduit le **pseudocode tunisien**
(programme du secondaire 🇹🇳) en **Python**, puis l'exécute directement dans le terminal.

A transpiler that translates **Tunisian high-school pseudocode** to **Python** and runs it.

Écrivez votre algorithme dans un fichier `.algo`, lancez une commande, et il s'exécute :

```console
$ python algotn.py exemple.algo
Combien de notes ? 3
...
Moyenne = 13.17
```

## Syntaxe

La syntaxe suivie est celle de l'extension VSCode
[**Algorithme en Pseudocode** (`algorithme-tn`)](https://marketplace.visualstudio.com/items?itemName=les-profs-d-info.algorithme-tn)
([code source](https://github.com/romoez/algo-tn-vscode)), qui fournit la coloration
syntaxique et les snippets pour les fichiers `.algo`. Utilisez les deux ensemble :
l'extension pour écrire, ce transpileur pour exécuter.

## Installation

Il faut uniquement **Python 3.10+** (aucune bibliothèque externe).

```console
git clone https://github.com/MajdLHB/algo-tn-compiler
cd algo-tn-compiler
python algotn.py exemple.algo
```

## Usage

```console
python algotn.py fichier.algo            # transpile + exécute
python algotn.py fichier.algo --show     # affiche aussi le code Python généré
python algotn.py fichier.algo --out f.py # sauvegarde le Python généré
python algotn.py fichier.algo --no-run   # transpile sans exécuter
```

## Ce qui est supporté

| Pseudocode | Python généré |
|---|---|
| `écrire("Bonjour", x)` (alias `afficher`) | `print("Bonjour", x, sep="")` |
| `lire(x)` (alias `saisir`) | lecture **typée selon le TDO** (`x : entier` → `int(input())`, avec re-saisie si valeur invalide ; `caractère` = exactement 1 caractère) |
| **tableaux de déclaration dessinés** (TDO / TDOG / TDOL) | lus où qu'ils soient dans le fichier ; un type non défini (ex : `entierr`) est une **erreur claire** |
| `fonction premier(n : entier) : booléen … retourner …` | `def premier(n): … return …` |
| `procédure permut(@x, @y : entier)` | passage par variable émulé : `x, y = permut(x, y)` |
| `x ← 5` (ou `x <- 5`) | `x = 5` |
| `si … alors / sinon si / sinon / fin_si` | `if / elif / else` |
| `pour i de 1 à n [pas p] faire … fin_pour` | `for i in range(1, n + 1)` — la borne finale est **atteinte** |
| `tant que … faire … fin_tant_que` | `while …:` |
| `répéter … jusqu'à cond` | `while True: … if cond: break` |
| `selon x … autres … fin_selon` | `match / case` |
| `t : tableau de 10 entier` (ou via TDNT) | tableau indexé **de 1 à n** comme en cours |
| `=  ≠  ≤  ≥  ∈` | `==  !=  <=  >=  in` |
| `et  ou  non  ouex` | `and  or  not  ^` |
| `div  mod  vrai  faux` | `//  %  True  False` |
| `// commentaire` et `/* … */` | ignorés |

Fonctions prédéfinies : `abs`, `ent`, `arrondi`, `racine_carrée`, `aléa`, `chr`, `ord`,
`majus`, `convch`, `valeur`, `estnum`, `long`, `pos`, `sous_chaîne`, `effacer`
(chaînes indexées à partir de 1, comme dans le programme officiel).

Les deux graphies sont acceptées : `écrire` / `Ecrire`, `fin_si` / `FinSi`,
`à` / `A`, etc.

### Déclarations (TDO)

Les déclarations sont lues pour construire une table des symboles : le type déclaré
détermine la conversion faite par `lire`.

```text
n : entier          // lire(n)    →  n = int(input())  (avec contrôle de saisie)
t : tableau de 10 réel   // lire(t[i]) →  t[i] = float(input())
```

## Pas encore supporté

- `enregistrement` (TDNT), fichiers texte et fichiers typés
- Les contributions sont les bienvenues !

## Exemples

Voir [`exemple.algo`](exemple.algo) (syntaxe de l'extension, accentuée),
[`exemple2.algo`](exemple2.algo) (syntaxe classique `Ecrire`/`FinSi`) et
[`exemple3.algo`](exemple3.algo) (fonctions, procédure avec `@`, TDOG/TDOL dessinés).

## Intégration VSCode

Deux PR ajoutent à l'extension `algorithme-tn` un bouton **▶ Exécuter**
([PR #7](https://github.com/romoez/algo-tn-vscode/pull/7)) et un **éditeur de
tableaux de déclaration** dans un onglet
([PR #8](https://github.com/romoez/algo-tn-vscode/pull/8)) — voir
[MajdLHB/algo-tn-vscode](https://github.com/MajdLHB/algo-tn-vscode) pour la
version empaquetée (`.vsix`).

## Licence

[MIT](LICENSE). La syntaxe du pseudocode est celle de l'extension
[`algorithme-tn`](https://github.com/romoez/algo-tn-vscode) (GPLv3), qui n'est pas
incluse dans ce dépôt.
