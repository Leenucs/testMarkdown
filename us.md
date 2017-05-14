# US Technique

## Problème
Historiquement, l'application Polen ne possédait qu'un seul assureur : ONEY.
A l'arrivée du second assureur COMPENSA, les spécificités ont commencé à arriver, générant des conditions d'affichage, de comportement, etc...

Cela se traduisait dans le code par des conditions "If Compensa".

Exemple :

```groovy
<g:if test="${projet.isCompensa()}">
    <g:textField name="telephoneMobile" value="${assure.telephoneMobile}" minlength="9" maxlength="9" placeholder="${message(code: 'screen.insuredperson.telephonemobile')} *"/>
</g:if>
<g:else>
    <g:textField name="telephoneMobile" value="${assure.telephoneMobile}" placeholder="${message(code: 'screen.insuredperson.telephonemobile')} *"/>
</g:else>

def destinataire = projet.isCompensa()?messageSource.getMessage("screen.documentsdownload.doc.compensa.mandat.nomDestinataire", null, locale):messageSource.getMessage("screen.documentsdownload.doc.oney.mandat.nomDestinataire", null, locale)
        data["nomDestinataire"] = destinataire.toUpperCase().take(27)
        data["nomAssureur"] = projet.isCompensa()?messageSource.getMessage("screen.documentsdownload.doc.compensa.mandat.adresseDestinataire", null, locale).take(27):messageSource.getMessage("screen.documentsdownload.doc.oney.mandat.adresseDestinataire", null, locale).take(27)
        if (projet.isCompensa()){
            // For Compensa, we use Compensa bank account number of the insured
            data["compteDestinataire"] = projet.refExterneCompensa.numeroBancaire
        } else {
            data["compteDestinataire"] = messageSource.getMessage("screen.documentsdownload.doc.oney.mandat.compteDestinataire", null, locale)
        }
```

L'idée était de nettoyer cette complexité et de proposer un paramétrage facilité, avant l'arrivée d'autres assureurs.

## Solution

## Implémentation
```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```groovy
def s = "JavaScript syntax highlighting";
```

```java
def s = "JavaScript syntax highlighting";
```

## Retour d'expérience

- [x] Logique conditionnelle déportée à un seul endroit.

[x] Paramétrage facilité lors de l'arrivée.

[X] Réduction des "if".



 :metal: :metal: :metal:

 ## Liens
** TODO ** US JIRA
