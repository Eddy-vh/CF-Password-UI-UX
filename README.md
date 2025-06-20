# CF-Password-UI-UX
## Convert Forms, moderniser les champs Nom d'utilisateur et Mot de passe

### Intro
Dans l'application **Convert Forms**, nous avons un champ de type "**password**" qui ne se diffère pas d'un champ de texte classique et qui n'offre pas la possibilité d'afficher temporairement la saisie et vérifier que l'on n'ait pas tapé un mot de passe erroné.

### To Do
Joomla proposant ceci par défaut sur son formulaire de connexion, nous pouvons nous servir de ses classes pour modifier le comportement du champ "**Password**" et styliser un champ de type text "**Nom d'utilisateur**" (username) en leur ajoutant les icônes représentatives.

Pour pouvoir obtenir les fonctionnalités des champs login et mot de passe, il faut modifier le champ de texte proposé par Convert Forms.
Tassos, le développeur de l'extension, indique dans [sa documentation](https://www.tassos.gr/docs/convert-forms/developers/override-form-ands-layouts-in-your-template#override_a_field_layout) la manière de surcharger les différents champs de l'application.

### Mise en place[^1]
Nous allons surcharger le champ de type **TEXT** afin d'y ajouter les fonctions pour les champs ciblés.

Rendez-vous vers le fichier original selon le chemin **/administrator/components/com_convertforms/layouts/fields/** et copiez le fichier **text.php**.

Rendez-vous ensuite dans le répertoire de surcharge de votre template selon le chemin **/templates/YOUR_TEMPLATE/html/layouts/com_convertforms/fields/** (si les répertoires n'existent pas, il faudra les créer) et collez-y le fichier copié.

Il faut ensuite modifier le fichier. Vous pouvez le faire à l'aide de tout éditeur ou plus simplement dans votre administration, accédez au lien de menu **Extensions - Templates - Templates** - cliquez sur votre template ouvrez ensuite l'arborescence **html/layouts/com_convertforms/fields/** et sélectionnez le fichier **text.php** afin d'afficher son code dans l'éditeur.

En toute fin du fichier, après la ligne 63, saisissez le bout de code suivant :
```PHP
<?php if ($field->type === 'password'): ?>
    <button type="button" class="btn btn-secondary cf-btn input-password-toggle" role="switch" aria-checked="false">
        <span class="icon-eye icon-fw" aria-hidden="true"></span>
        <span class="visually-hidden">Afficher le mot de passe</span>
    </button>
<?php endif; ?>
<?php if (isset($field->class) && strpos($field->class, 'username') !== false): ?>
    <span class="input-group-text" title="Identifiant" style="justify-content:center;">
    <span class="icon-user icon-fw" aria-hidden="true"></span>
    </span> 
<?php endif; ?>
```
Enregistrez ces modifications et fermez le fichier.

Ce bout de code permet de vérifier que le champ soit de type password ou de type texte avec la classe **username** (que nous devrons alors indiquer dans les paramètres du champ de formulaire "**Nom d'utilisateur**".

Si vous avez préparé un formulaire et qu'il puise être affiché en front, vous pouvez voir que le champ de type mot de passe a été modifié, mais qu'il ne ressemble pas du tout à ce qui était espéré.
En effet, l'icône d'œil a bien été intégrée, mais qu'elle se trouve sous le mot de passe. Il faudra ajouter un peu de **style CSS** afin que le rendu soit celui attendu.
Dans le cas d'un champ "Nom d'utilisateur" auquel vous auriez ajouté une classe "username", le comportement de l'icône sera semblable à celle du mot de passe mais elle n'aura aucune autre fonction que le rendu visuel.


Dans l'arborescence de votre template, vous nécessiterez d'un fichier CSS personnalisé qui ne pourra être écrasé à la prochaine mise à jour du template. Selon votre template, ce fichier doit se nommer "**user.css**" ou "**custom.css**" (voyez la documentation de votre template).

Dans ce fichier vous ajouterez le code CSS suivant :
```CSS
/* format Password / Username + Button */

.convertforms div[data-type="password"] div.cf-control-input,
.convertforms div[data-name="username"] div.cf-control-input {
    flex-direction: row;
    gap:0;
    flex-wrap:wrap;
}
.convertforms div[data-type="password"] input[type="password"], .convertforms div[data-type="password"] input[type="text"],
.convertforms div[data-name="username"] input {
    border-top-right-radius:0;
    border-bottom-right-radius:0;
    width:calc(100% - 54px);
}

.convertforms div[data-type="password"] button,
.convertforms div[data-name="username"] input + span {
    width:54px;
    background:#f9fafb;
    border: 1px solid lightgrey !important;
    border-radius:.25rem; /* adaptez à votre formulaire en cas de besoin */
    border-top-left-radius:0 !important;
    border-bottom-left-radius:0 !important;
    border-left:none !important;
    padding: .6rem 1rem !important;
    color: #353B41 !important;
}

.convertforms div[data-type="password"] div.cf-control-input-desc,
.convertforms div[data-name="username"] div.cf-control-input-desc {
	margin-top:10px;
}
```
Après cet ajout, enregistrez votre fichier, videz les caches et observez maintenant les champs de votre formulaire après avoir actualisé la page. Saisissez quelques caractères dnas le champ de mot de passe et cliquez l'icône œil afin de voir la saisie en mode texte. Cliquez l'icône œil barré pour revenir à l'état "Mot de passe" hachuré.

Pour obtenir l'icône du nom d'utilisateur pour le champ correspondant, il est important de lui donner la **classe CSS de saisie** "**username**" lors de l'édition de ce champ de type texte.


Si vous souhaitez obtenir le visuel identique dans l'administration lors de l'édition d'un formulaire, vous devrez répéter les opérations décrites ci-dessus pour le template d'administration Atum.


[^1]:Les fichiers nécessaires à cette adaptation sont disponibles au téléchargement, ils doivent être placés selon l'arborescence des répertoires dézippés.
