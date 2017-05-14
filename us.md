# US Technique

## Problème
Historiquement, l'application Polen ne possédait qu'un seul assureur : ONEY.
A l'arrivée du second assureur COMPENSA, les spécificités ont commencé à arriver, générant des conditions d'affichage, de comportement, etc...

Cela se traduisait dans le code par des conditions "If Compensa".

Exemples :

>Conditions sur l'affichage d'un champ de formulaire, dans la vue
```groovy
<g:if test="${projet.isCompensa()}">
    <g:textField name="telephoneMobile" value="${assure.telephoneMobile}" minlength="9" maxlength="9" placeholder="${message(code: 'screen.insuredperson.telephonemobile')} *"/>
</g:if>
<g:else>
    <g:textField name="telephoneMobile" value="${assure.telephoneMobile}" placeholder="${message(code: 'screen.insuredperson.telephonemobile')} *"/>
</g:else>
```
<br>

>Conditions sur une valeur de 
```groovy
if (projet.isCompensa()){
    // For Compensa, we use Compensa bank account number of the insured
    data["compteDestinataire"] = projet.refExterneCompensa.numeroBancaire
} else {
    data["compteDestinataire"] = messageSource.getMessage("screen.documentsdownload.doc.oney.mandat.compteDestinataire", null, locale)
}
```

L'idée était de nettoyer cette complexité et de proposer un paramétrage facilité, avant l'arrivée d'autres assureurs.

## Solution
L'idée est de créer une classe utilitaire permettant d'appeler des méthodes spécifiques retournant une valeur en fonction de l'assureur.

Il faut pour cela une classe abstraite permettant de définir des valeurs par défaut. Elle sera ensuite surchargée au besoin par les assureurs.

## Implémentation

![Diagramme](/images/Diagramme.png)

### AssureurHelper:
Cette classe contient les méthodes statiques permettant de récupérer une valeur paramétrée pour un assureur. On lui passe généralement l'objet "projet" permettant de déterminer dans quel contexte on se trouve.

### AbstractAssureur

### AssureurAxa

### AssureurCompensa

### AssureurOney


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

- [x] Paramétrage facilité lors de l'arrivée d'un nouvel assureur.

- [x] Réduction des "if".

 :metal: :metal: :metal:

 ## Liens
** TODO ** US JIRA
