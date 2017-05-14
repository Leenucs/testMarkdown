# US Technique

## Problème
Au départ, l'application Polen ne possédait qu'un seul assureur : ONEY.
A l'arrivée du second assureur COMPENSA, les spécificités ont commencé à apparaître, générant des conditions d'affichage, de comportement, etc...

Cela se traduisait dans le code par des conditions "If Compensa".

Exemples :

>Conditions sur l'affichage d'un champ de formulaire, côté vue
```groovy
<g:if test="${projet.isCompensa()}">
    <g:textField name="telephoneMobile" value="${assure.telephoneMobile}" minlength="9" maxlength="9" placeholder="${message(code: 'screen.insuredperson.telephonemobile')} *"/>
</g:if>
<g:else>
    <g:textField name="telephoneMobile" value="${assure.telephoneMobile}" placeholder="${message(code: 'screen.insuredperson.telephonemobile')} *"/>
</g:else>
```
<br>

>Conditions sur un remplissage de champ PDF, côté contrôleur
```groovy
if (projet.isCompensa()){
    // For Compensa, we use Compensa bank account number of the insured
    data["compteDestinataire"] = projet.refExterneCompensa.numeroBancaire
} else {
    data["compteDestinataire"] = messageSource.getMessage("screen.documentsdownload.doc.oney.mandat.compteDestinataire", null, locale)
}
```

Le but était de nettoyer cette complexité et de proposer un paramétrage simplifié, avant l'arrivée d'autres assureurs.

## Solution
L'idée est d'utiliser une classe utilitaire permettant d'appeler des méthodes statiques, spécifiques à chaque assureur.

Il faut également une classe abstraite permettant de définir des valeurs par défaut. Elle sera ensuite surchargée au besoin par les classes assureurs.

## Implémentation

![Diagramme](/images/Diagramme.PNG)

### AssureurHelper:
Cette classe contient les méthodes statiques permettant de récupérer une valeur paramétrée pour un assureur. On lui passe généralement l'objet "projet" permettant de déterminer dans quel contexte on se trouve.

```groovy
/**
* Indique la valeur à indiquer dans le champ "compte" du mandat
*/
static getCompteDestinataireMandat(Projet projet){
    InternalHelper.getAssureur(projet.optionChoisie.assureur.id).getCompteDestinataireMandat(projet)
}
```

InternalHelper est défini au sein de la classe AssureurHelper et permet de déclarer les assureurs et récuperer la bonne instance.

```groovy
static enum InternalHelper {
    INSTANCE
    Map<Long, AbstractAssureur> assureurs
    private InternalHelper() {
        assureurs = [(Constants.COMPENSA_ID): new AssureurCompensa(Constants.ONEY_INSURANCE_ID): new AssureurOney(Constants.ONEY_LIFE_ID): new AssureurOney(), (Constants.AXA_Inew AssureurAxa()]
    
    static getAssureur(Long idAssureur){
        if (!idAssureur || !INSTANCE.assureurs.containsKey(idAssureur)){
            throw new IllegalStateException("L'id " + idAssureur + " null ou non configuré dans la liste des assureurs (claInternalHelper)")
        }
        else {
            InternalHelper.INSTANCE.assureurs.get(idAssureur)
        }
    }
}
```

### AssureurHelperSpec:
Classe de test d'AssureurHelper

### AbstractAssureur
C'est la classe abstraite définissant les valeurs par défaut.
```groovy
def getCompteDestinataireMandat(Projet projet){
    messageSource.getMessage("screen.documentsdownload.doc.oney.mandat.compteDestinataire", null, LocaleContextHolder.getLocale())
}
```

### AssureurOney, AssureurAxa, AssureurCompensa, 
Les classes Assureur, c'est ici que l'on peut surcharger les méthodes et spécifier les particularités des assureurs si besoin.
```groovy
def getCompteDestinataireMandat(Projet projet){
    projet.refExterneCompensa.numeroBancaire
}
```

L'exemple vu précédemment sur le contrôleur peut maintenant s'écrire :

```groovy
    data["compteDestinataire"] = AssureurHelper.getCompteDestinataireMandat(projet);
```

## Retour d'expérience

- [x] Spécificités des assureurs déportées à un seul endroit.

- [x] Paramétrage facilité lors de l'arrivée d'un nouvel assureur.

- [x] Nettoyage des "if assureur" côté Vue et Contrôleurs.

 ## Liens
** TODO ** US JIRA
** TODO ** Code Polen
