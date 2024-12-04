Pour gérer les champs à vider lorsque vous utilisez des input classiques ou des composants Coral UI comme Coral.RichText, vous pouvez adapter le JavaScript en fonction des types de champs que vous souhaitez cibler. Voici une solution détaillée.


---

Étape 1 : Adapter le JavaScript pour input et Coral.RichText

Voici un script qui gère à la fois les champs input et les champs Coral UI comme Coral.RichText :

(function ($, Granite) {
    $(document).on("click", ".clear-fields-button-a", function () {
        // Trouver le FieldSet A
        const fieldSetA = $(this).closest(".coral-FieldSet, .coral-Form-fieldset");

        // Parcourir et réinitialiser les champs dans ce FieldSet
        fieldSetA.find("input, coral-richtext").each(function () {
            const field = $(this);

            // Réinitialiser les champs texte classiques
            if (field.is("input")) {
                field.val("");
            }

            // Réinitialiser les Coral RichText
            if (field.is("coral-richtext")) {
                field[0].value = ""; // Utiliser la propriété native de Coral
            }
        });

        Granite.UI.Foundation.Notification.show("success", "Champs du FieldSet A réinitialisés !");
    });

    $(document).on("click", ".clear-fields-button-b", function () {
        // Trouver le FieldSet B
        const fieldSetB = $(this).closest(".coral-FieldSet, .coral-Form-fieldset");

        // Parcourir et réinitialiser les champs dans ce FieldSet
        fieldSetB.find("input, coral-richtext").each(function () {
            const field = $(this);

            // Réinitialiser les champs texte classiques
            if (field.is("input")) {
                field.val("");
            }

            // Réinitialiser les Coral RichText
            if (field.is("coral-richtext")) {
                field[0].value = ""; // Utiliser la propriété native de Coral
            }
        });

        Granite.UI.Foundation.Notification.show("success", "Champs du FieldSet B réinitialisés !");
    });
})(jQuery, Granite);


---

Étape 2 : Points importants dans le script

1. input classique :

Les champs input sont ciblés directement avec field.is("input").

Leur contenu est vidé avec field.val("").



2. Coral.RichText :

Les composants Coral RichText sont détectés avec field.is("coral-richtext").

Le contenu est réinitialisé avec field[0].value = "".



3. Scope limité au FieldSet :

Le bouton agit uniquement sur les champs présents dans le FieldSet parent grâce à .closest().



4. Notifications utilisateur :

Une notification dynamique informe l'utilisateur des actions effectuées.





---

Étape 3 : Tester

1. Intégrez ce script dans votre clientlib (par exemple, dans clearFields.js).


2. Ajoutez les champs nécessaires (input ou coral-richtext) dans vos FieldSet respectifs.


3. Testez les boutons Clear A et Clear B pour vérifier qu'ils vident correctement leurs champs respectifs.




---

Exemple d’un dialogue avec input et Coral.RichText

<content jcr:primaryType="cq:DialogContent">
  <items jcr:primaryType="cq:WidgetCollection">
    
    <!-- Premier FieldSet -->
    <fieldSetA
      jcr:primaryType="cq:Widget"
      xtype="fieldset"
      title="FieldSet
