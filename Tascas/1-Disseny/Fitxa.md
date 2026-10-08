# Fitxa 1 — Anàlisi inicial de MusicCloud

## Objectiu

MusicCloud necessita reorganitzar la seva infraestructura informàtica. Abans d'instal·lar o configurar cap servei, cal entendre:

- qui treballa a l'empresa;
    
- quines funcions té cada persona;
    
- quins recursos existeixen;
    
- qui necessita accedir a cada recurs;
    
- com podem gestionar aquests accessos de manera eficient.
    

---

# 1. Conèixer MusicCloud

Consulta la informació disponible sobre els departaments, treballadors i perfils d'usuari de MusicCloud.

Completa la taula següent.

|Persona|Departament|Funció / responsabilitat|Necessita privilegis especials? Per què?|
|---|---|---|---|
|Aina Ciurans|Direcció|Gestió general de l'empresa|Sí, accés L/E a tots els recursos de Direcció, inclosa la carpeta confidencial|
|Rut Tornil|Direcció|Gestió general de l'empresa|Sí, mateix motiu que Aina Ciurans|
|Dídac Gassó|Administració|Factures, contractes i documentació interna|No, usuari estàndard del departament|
|Laia Macias|Administració (cap)|Factures, contractes i doc. interna + coordinació del departament|Sí, com a cap necessita accés a gestio_departament|
|Estel Birosta|Suport tècnic|Manteniment de sistemes i incidències|No, usuari estàndard|
|Aina Zuriguel|Suport tècnic|Manteniment de sistemes i incidències|No, usuari estàndard|
|Lluïsa Richart|Suport tècnic (cap)|Manteniment i incidències + coordinació|Sí, accés a gestio_departament i scripts|
|Roser Alberch|Producció musical|Gestió de continguts musicals|No|
|Guillem Adella|Producció musical|Gestió de continguts musicals|No|
|Meritxell Reglat|Producció musical (cap)|Gestió de continguts + coordinació|Sí, accés a gestio_departament|
|Alícia Monclús|Producció musical|Gestió de continguts musicals|No|
|Carles Molins|Producció musical|Gestió de continguts musicals|No|
|Eulàlia Galcera|Producció musical|Gestió de continguts musicals|No|
|Talia Costas|Informàtica (cap)|Suport sistema informàtic + administració|Sí, accés a backups, logs, configuracions|
|Alex Soriano|Informàtica|Suport sistema informàtic|Sí, accés tècnic ampli (rol d'informàtica)|
|Pere Espinalt|Extern|Col·laborador extern|Accés limitat i temporal (només intercanvi)|
|Neus Bages|Extern|Col·laborador extern|Accés limitat i temporal (només intercanvi)|

### 1.1. Reflexió

Quines diferències observes entre un **treballador**, un **departament** i una **funció o responsabilitat**?

---
Un treballador és la persona concreta, amb nom i cognoms.
---
Un departament és l'agrupació organitzativa a la qual pertany aquesta persona (Administració, Producció musical...).
---
Una funció o responsabilitat és el rol que exerceix aquella persona dins del departament, i és el que realment determina quins permisos necessita: dues persones poden pertànyer al mateix departament però tenir funcions diferents (per exemple, un usuari estàndard i el cap de departament), i per tant necessitar nivells d'accés diferents encara que comparteixin departament.
---
Hi ha persones que, pel seu càrrec o funció, necessiten accessos diferents dels altres membres del seu departament?

☐ Sí

Posa'n algun exemple:

---
Laia Macias (cap d'Administració) té accés a gestio departament, que Dídac Gassó no té. Talia Costas (Informàtica) té accés als sistemes, més ampli que un usuari estàndard del mateix departament.
---


# 2. Recursos de l'empresa

Analitza l'estructura d'informació de MusicCloud.

Classifica alguns dels recursos següents segons la seva finalitat.

|Recurs|Qui creus que l'hauria d'utilitzar?|Per a què?|
|---|---|---|
|`/empresa/comu/intercanvi`|Tothom (treballadors i externs)|Intercanvi temporal de documents, també amb externs|
|`/empresa/comu/comunicats`|Tots els treballadors (lectura), Direcció (L/E) i menys els externs (cap acess)|Consultar i publicar comunicats interns|
|`/empresa/departaments/administracio/compartida`|Departament d'Administració i Direcció només lectura|Documents de treball habituals del departament|
|`/empresa/departaments/administracio/gestio_departament`|Cap d'Administració (Laia Macias)|Gestió interna del departament, restringit al responsable|
|`/empresa/projectes/campanya_estiu`|Usuaris assignats al projecte (de diversos departaments)|Treball conjunt en el projecte transversal|
|`/empresa/administracio_sistema/backups`|Departament d'Informàtica|Gestió tècnica de les còpies de seguretat|

---

# 3. Qui ha de poder fer què?

Per a cada situació, indica quin nivell d'accés consideres adequat.

Utilitza:

- **NA** → sense accés
    
- **L** → lectura
    
- **L/E** → lectura i escriptura
    
- **ADM** → administració
    

No busquis encara una solució tècnica. Pensa només en les necessitats de l'empresa.

|Situació|Accés proposat|Justificació|
|---|---|---|
|Dídac accedeix a la carpeta compartida d'Administració|L/E|Treballador habitual del departament|
|Laia accedeix a la gestió del departament d'Administració|L/E|És la cap del departament|
|Pere, treballador extern, accedeix als comunicats interns|NA|És extern; els comunicats interns no li pertoquen|
|Talia accedeix als backups del sistema|ADM|Responsable d'Informàtica, gestiona el sistema|
|Un membre de Producció musical accedeix a la carpeta d'Administració|NA|No pertany al departament, sense relació amb la feina|
|Un participant de `campanya_estiu` accedeix als fitxers del projecte|L/E|Cal treballar-hi, però només si hi està assignat al projecte|


---

# 4. Primer problema: com assignem els permisos?

Imagina que MusicCloud té només quatre treballadors:

- Anna
    
- Biel
    
- Carla
    
- David
    

Tots quatre treballen al mateix departament i necessiten accedir a la mateixa carpeta.

Una possible solució seria configurar:

```text
Anna  → lectura/escriptura
Biel  → lectura/escriptura
Carla → lectura/escriptura
David → lectura/escriptura
```

### 4.1.

Què passaria si l'empresa tingués **100 treballadors** amb el mateix tipus d'accés?

---
Amb 100 treballadors caldria repetir 100 vegades la mateixa configuració individual. Això no només fa perdre molt de temps a l'administrador, sinó que fa pràcticament impossible garantir que tothom acabi tenint exactament els mateixos permisos: n'hi hauria prou amb un oblit o un pas fet diferent per a algú perquè el seu accés quedés inconsistent respecte a la resta, i seria molt difícil detectar-ho o auditar-ho més endavant.
---

### 4.2.

Què passaria cada vegada que s'incorporés una persona nova?

---
Caldria repetir tot el procés manual de configuració de permisos i axo es molt lenta i perdes el temps un proces tan simple i repetetiu.
---

### 4.3.

Què passaria quan una persona canviés de departament?

---
El principal risc és que li quedin actius els accessos del departament anterior perquè ningú els ha retirat, podent consultar informació que ja no li pertoca. A més, si també s'oblida donar-li els permisos nous, no podrà treballar amb normalitat des del primer dia.
---

### 4.4.

Proposa una manera de gestionar aquestes persones conjuntament.

No cal que coneguis encara el nom tècnic de la solució.

---
Podem imaginar com una clau que serveix per obrir una mateixa porta: en lloc de fer una clau diferent per a cada persona, en fem una de sola i la donem a tothom que forma part d'un mateix "grup". Així, en lloc de configurar el permís persona per persona, creem un grup, hi afegim les persones que necessiten el mateix accés, i donem el permís una única vegada a tot el grup. Qui hi entra ja té l'accés automàticament, i qui en surt el perd, sense haver de tocar res persona per persona.
---

---

---

# 5. Canvis a MusicCloud

Ara es produeixen aquests tres canvis:

### Cas A

Dídac deixa Administració i passa a Producció musical.

Quins accessos hauria de perdre?

---
Els accessos a `/empresa/departaments/administracio/compartida` i `/empresa/departaments/administracio/documentacio_interna`.
---

Quins accessos hauria d'obtenir?

---
Els accessos a `/empresa/departaments/produccio_musical/compartida`, artistes i cataleg.
---

### Cas B

S'incorpora una nova treballadora al departament d'Administració.

Quins accessos caldria configurar?

---
Accés als recursos comuns (comu/intercanvi, comu/plantilles, comu/comunicats) i als recursos del departament d'Administració (compartida i documentacio_interna), amb el mateix nivell que la resta de treballadors del departament.
---

### Cas C

Pere Espinalt deixa de col·laborar amb MusicCloud.

Què hauríem de fer amb els seus accessos?

---
Donar de baixa el seu compte i revocar immediatament tots els seus accessos (incloent-hi comu/intercanvi), per evitar que pugui continuar entrant al sistema un cop ja no hi col·labora.
---

# 6. Busquem una solució millor

Suposa ara que podem crear conjunts de persones que comparteixen unes mateixes necessitats d'accés.

Per exemple:

```text
Administració
    ├── Dídac
    ├── Laia
    └── Roser
```

I podem donar permisos directament al conjunt:

```text
Administració → carpeta_administracio → L/E
```

### 6.1.

Quin avantatge té aquesta solució respecte a donar permisos persona per persona?

---
Es modifica el permís una sola vegada al conjunt i afecta automàticament tothom qui en forma part, en lloc de repetir el canvi persona per persona.
---

### 6.2.

Si Dídac passa d'Administració a Producció musical, què caldria modificar?

---
Només cal treure'l del conjunt "Administració" i afegir-lo al conjunt "Producció musical".
---

### 6.3.

Com anomenaries aquests conjunts de persones?

---
No se, "grups d'usuaris ADM" pot ser.
---

# 7. Primera proposta per a MusicCloud

A partir de l'organització de l'empresa, proposa els primers conjunts de persones que crearies.

**No cal trobar encara la solució definitiva.**

|Nom proposat|Qui hi pertanyeria?|Per què existeix aquest conjunt?|
|---|---|---|
|GRP.Direccio|Aina Ciurans, Rut Tornil|Accés total als recursos de Direcció|
|GRP.Administracio|Dídac Gassó, Laia Macias|Accés al departament d'Administració|
|GRP.SuportTecnic|Estel Birosta, Aina Zuriguel, Lluïsa Richart|Accés al departament de Suport tècnic|
|GRP.ProduccioMusical|Roser Alberch, Guillem Adella, Meritxell Reglat, Alícia Monclús, Carles Molins, Eulàlia Galcera|Accés al departament de Producció musical|
|GRP.Informatica|Talia Costas, Alex Soriano|Accés ADM als sistemes|
|GRP.Externs|Pere Espinalt, Neus Bages|Accés limitat, només a recursos d'intercanvi|

---

# 8. Cas que complica el model

Laia treballa al departament d'Administració, però també és la responsable del departament.

És suficient que pertanyi només al conjunt `Administració`?

☐ No

Per què?

---
No. Com a cap, necessita permisos addicionals (accés a gestio_departament o GRP) que la resta del departament no té. Un sol grup no permet distingir rols diferents dins del mateix departament.
---

Quina possible solució proposes?

---
Que Laia pertanyi a dos grups alhora GRP_Administracio (accés estàndard del departament) i un grup addicional, per exemple GRP_CapsDepartament o GRP_CapAdministracio, amb els permisos extra de gestió del departament.
---

# 9. Un altre cas

Diverses persones de departaments diferents participen temporalment en el projecte:

```text
Campanya Estiu
```

Creus que hauríem de canviar-les de departament?

☐ No

Si no, com podríem donar-los accés als recursos del projecte?

---
Creem un grup nou només per al projecte, hi posem tothom qui hi participa (siguin del departament que siguin) i li donem accés a la carpeta del projecte. Cadascú segueix formant part també del grup del seu departament: ara, simplement, pertany a dos grups alhora.
---

# 10. Conclusions

Completa les frases amb les teves paraules.

### Usuari

Un usuari representa: Una persona (interna o externa) que pot identificar-se davant el sistema i accedir-hi els recursus  permitit del sistema.

---

### Recurs

Un recurs és: Qualsevol element carpeta, fitxer, servei, etc... sobre el qual es poden aplicar permisos d'accés (usuaris).

---

### Permís

Un permís determina: Quines accions lectura, escriptura, administració, etc... pot fer un usuari o un grup sobre un recurs concret (exple un departament).

---

### Grup

Un grup serveix per: Donar els mateixos permisos a tothom qui els necessita d'un sol cop, en lloc d'anar persona per persona.

---

---

# 11. Regla de mínim privilegi

Analitza aquesta afirmació:

> Un usuari només hauria de tenir els permisos estrictament necessaris per realitzar la seva feina.

Explica amb les teves paraules què significa.

---
Significa que a cada usuari se li han de donar només els accessos que necessita per fer la seva feina, ni més ni menys.
---

Posa un exemple relacionat amb MusicCloud.

---
Per exemple un membre de Producció musical no necessita accés a la carpeta de Direcció ni als backups del sistema; només el departament d'Informàtica ha de tenir accés ADM als backups.
---

---

# 12. Pregunta final

Imagina que demà MusicCloud passa de 14 treballadors a 500.

Quina de les dues estratègies consideres més adequada?


☐ Organitzar els usuaris segons les seves necessitats i assignar permisos a aquests conjunts.

Justifica la resposta.

---
Amb 500 treballadors, assignar permisos un per un seria inviable de gestionar i molt costós en temps i molt propens a errors i inconsistències, Organitzar els usuaris en grups segons les seves necessitats d'accés permet escalar la gestió, mantenir-la coherent i actualitzar-la fàcilment cada vegada que algú entra a l'empresa, canvia de departament o en marxa. 
---
En resum, nesesitem un AD o Samba
---

Jo **no faria obligatori que acabessin tota la fitxa abans d'explicar res**. La utilitzaria de manera sincronitzada amb la classe:

**0–40 min:** apartats 1–3 → analitzen MusicCloud i els accessos.  
**40–65 min:** apartats 4–5 → apareix el problema de gestionar permisos individualment.  
**65–85 min:** explicació curta de **usuari, grup, recurs, permís i mínim privilegi**.  
**85–110 min:** apartats 6–9 → apliquen immediatament el concepte de grup.  
**110–120 min:** apartats 10–12 → revisió i tancament.

Hi ha una decisió pedagògica important: a l'apartat 4 **no utilitzo la paraula “grup” fins que l'alumnat ha intentat resoldre el problema**. Això encaixa molt millor amb el cicle que vols seguir: primer tenen el problema, després apareix la necessitat i només aleshores introdueixes el concepte teòric.
