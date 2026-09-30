# Fitxa 2 — Organització del servei de directori de MusicCloud

## Objectiu

En aquesta sessió hem decidit com organitzar els diferents objectes de MusicCloud dins d'un servei de directori.

Aquesta fitxa forma part de la **documentació de disseny del sistema**. Les decisions que hi indiquis s'utilitzaran posteriorment durant la implantació.
# 1. Objectes que hem de gestionar

MusicCloud necessita gestionar de manera centralitzada diferents tipus d'objectes.

Indica quins tipus d'objectes consideres que ha de contenir el servei de directori.

|Tipus d'objecte|Exemples a MusicCloud|
|---|---|
|Usuaris| Aina Ciurans, Laia Macias, Talia Costas... |
|Grups| Caps.Departament, Admins Sistema, Campanya.Estiu |
|Equips (ordinadors)| PC.Direccio-1, PC.Informatica-3 |
|Servidors| Serv.Fixers, Serv.Web, Serv.BD |
|Comptes d'aplicacions o serveis| Compte del servei de còpies de seguretat, compte de connexió a la base de dades |

Hi afegiries algun altre tipus d'objecte?

---
- Ordinadors o Dispositius (equips de cada treballador)
- Recursos compartits (impressores, carpetes de xarxa)
---

# 2. Organització mitjançant unitats organitzatives

Proposa les **unitats organitzatives (OU)** principals que utilitzaries a MusicCloud.

|OU|Què contindrà?|Per què la crees?|
|---|---|---|
| Usuaris | Subcarpetes per departament (Direcció, Administració... etc.) | Reflecteix l'organigrama real i permet aplicar polítiques diferents per departament |
| Equips | Ordinadors dels treballadors | Necessiten polítiques de seguretat pròpies, diferents dels usuaris |
| Servidors | Els servidors de l'empresa | Requereixen polítiques molt més restrictives que un ordinador normal |
| Grups | Tots els grups de seguretat | Per tenir-los localitzats i fàcils de gestionar |
| Aplicacions | Comptes de servei | Solen necessitar configuracions especials (contrasenyes que no caduquen, etc.) |

## 2.1. Organització dels usuaris

Dibuixa l'estructura que utilitzaries per organitzar els usuaris de MusicCloud.

```text
MusicCloud
│
└── Usuaris
    ├── Direccio
    ├── Administracio
    ├── Suport_Tecnic
    ├── Produccio_Musical
    ├── Informatica
    └── Externs
```

---

# 3. OU o grup?

Indica quina opció utilitzaries principalment en cada cas.

|Necessitat|OU|Grup|
|---|:-:|:-:|
|Organitzar els treballadors d'Administració| ☐ |  |
|Donar accés a la carpeta d'Administració|  | ☐ |
|Organitzar els ordinadors clients| ☐ |  |
|Identificar les persones que participen en Campanya Estiu|  | ☐ |
|Organitzar els servidors| ☐ |  |
|Donar privilegis als administradors del sistema|  | ☐ |
|Organitzar els comptes utilitzats per aplicacions| ☐ |  |

### Explica amb les teves paraules la diferència principal entre una OU i un grup.

**OU:** és el lloc on "viu" l'objecte. Cada usuari, ordinador o servidor pertany a una única OU, i serveix per organitzar l'estructura i aplicar-hi polítiques (per exemple, totes les OU de Direcció tenen la mateixa configuració de seguretat)

---

---

**Grup:** és una etiqueta que pots enganxar a qualsevol objecte, independentment d'on estigui ubicat. Un mateix usuari pot portar-ne moltes alhora (pertànyer a diversos grups), i serveix bàsicament per donar permisos o identificar qui participa en què, per exemple un departament amb permisus només nesesarias

---

---

---

# 4. Un mateix usuari: ubicació i pertinença

Considera aquest cas:

**Dídac Gassó**

- treballa a Administració;
    
- participa en el projecte Campanya Estiu.
    

Indica:

**En quina OU ubicaries el seu compte?** MusicCloud > Usuaris > Administracio, perquè aquesta és la seva ubicació estructural, el seu departament fix.

---

**A quins grups podria pertànyer?** Administracio (accés als recursos del departament) i Campanya_Estiu (accés temporal als recursos d'aquest projecte concret).

---

---

### Per què no és contradictori que estigui en una OU però pertanyi a diversos grups?

---
Perquè l'OU respon a on visc (una sola adreça) i el grup respon a "de quins clubs sóc membre" (en pots ser de diversos alhora). En Dídac té una única ubicació fixa dins l'organigrama, però pot collaborar en tants projectes o tenir tants permisos afegits com calgui, sense que això mogui la seva ubicació de base.
---

---

# 5. Servei de directori

Explica breument què entens per **servei de directori**.

---
Una base de dades centralitzada i jeràrquica que emmagatzema informació sobre els recursos de la xarxa (usuaris, grups, ordinadors, servidors...) i permet consultar-los i gestionar-los de manera unificada.
---

Quin problema resol a MusicCloud?

---
Sense servei de directori, cada ordinador i servei tindria el seu propi pany i clau. Amb un servei de directori hi ha un sol pany mestre: un usuari, una contrasenya, i accés a tot des d'un únic lloc. Si canvia alguna cosa, es canvia un cop i s'aplica arreu, es la manera més apropiat els 14 taballadors.
---

---

# 6. LDAP

Completa les frases següents.

**LDAP és:** LDAP (Lightweight Directory Access Protocol) és un protocol d'aplicació estàndard per accedir i gestionar serveis de directori distribuïts, funciona sobre TCP/IP amb un model client-servidor (port 389). 

---

**LDAP no és:** no es un servel rollo Access Directory, utilitza LDAP com a protocol de comunicació. LDAP és el llenguatge; AD és un dels "parlants".

---

Indica si les afirmacions són certes o falses.

|Afirmació|C|F|
|---|:-:|:-:|
|LDAP és sinònim d'Active Directory||☐|
|LDAP permet accedir i consultar informació d'un directori|☐||
|OpenLDAP és una implementació d'un servei de directori|☐||
|Active Directory utilitza LDAP, entre altres tecnologies|☐||

---

# 7. DIT de MusicCloud

Dibuixa la proposta final de **Directory Information Tree (DIT)** de MusicCloud.

Ha de mostrar, com a mínim:

- usuaris;
    
- grups;
    
- equips;
    
- servidors;
    
- comptes d'aplicacions o serveis;
    
- les subdivisions que consideris necessàries.
    

```text
MusicCloud
│
├── OU_Usuaris
│   ├── OU_Direccio
│   ├── OU_Administracio
│   ├── OU_SuportTecnic
│   ├── OU_ProduccioMusical
│   ├── OU_Informatica
│   └── OU_Externs
├── OU_Grups
│   ├── Grup_Caps_Departament
│   ├── Grup_Admins_Sistema
│   └── Grup_Campanya_Estiu
├── OU_Equips
├── OU_Servidors
└── OU_Aplicacions
```

---

# 8. Justificació del disseny

Escull **dues decisions** del teu DIT que consideris importants i justifica-les.

### Decisió 1

---

**Justificació:** els servidors necessiten normes de seguretat molt més estrictes (poca gent hi pot accedir, cal anar amb compte amb les actualitzacions) que els ordinadors normals; tenint-los separats, evites que una configuració pensada per a un ordinador de treball acabi afectant per error un servidor important.

---

---

### Decisió 2

---

**Justificació:** els projectes són temporals i hi participa gent de diversos departaments; si haguéssim fet una carpeta nova, hauríem hagut de moure persones del seu lloc habitual, un grup, en canvi, permet donar accés temporal sense moure ningú de la seva ubicació de base.

---

---

---

# 9. Comprovació final

Respon breument.

### a) Per què no seria una bona idea guardar tots els usuaris, grups, equips i servidors al mateix nivell sense organitzar-los?

---
Faria impossible de gestionar a mesura que creix l'empresa

---

### b) Per què no hauríem d'utilitzar les OU per substituir els grups de permisos?

---
cada persona només pot estar en una carpeta, però sovint necessita formar part de diverses coses alhora (el seu departament, un projecte, un rol especial)
---

### c) Si MusicCloud passa de 14 a 500 treballadors, quina característica del disseny que has fet avui facilitarà més l'administració?

---
Tenir les carpetes organitzades per departament des del principi cada treballador nou només s'ha de col·locar a la carpeta del seu departament i ja hereta automàticament les normes que li toquen, sense configurar-lo un per un

---

---

# Documentació final del sistema

A partir de les decisions preses durant la sessió, deixa definida la proposta que utilitzarem inicialment per a MusicCloud.

## Estructura d'unitats organitzatives

```text
MusicCloud
│
├── Usuaris (cada departamen)
├── Grups
├── Equips
├── Servidors
└── Aplicacions
```

## Criteri utilitzat per organitzar els objectes

---
Hem agrupat els usuaris, ordinadors i servidors segons el departament al qual pertanyen, és a dir, calcant l'organigrama real de l'empresa, de manera que la carpeta de cadascú coincideix amb l'equip on treballa de veritat.

---

## Criteri utilitzat per diferenciar OU i grups

---
OU per a la ubicació estructural i permanent; grups per a la pertinença funcional i temporal (permisos, projectes, rols transversals)

---