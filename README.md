# bank-config-service
Pour gérer un dialogue AEM avec deux tabs, où une tab devient grisée en fonction de l'état d'un champ dans l'autre tab, voici la solution adaptée :


---

Fonctionnalités demandées :

1. Griser Tab 1 si un champ nommé titre2 dans Tab 2 est rempli.


2. Griser Tab 2 si un champ nommé titre1 dans Tab 1 est rempli.




---

1. CSS pour désactiver les tabs

Ajoutez une classe CSS pour griser les tabs et désactiver leur interaction.

<style>
    .disabled-tab {
        pointer-events: none; /* Désactive les clics */
        opacity: 0.5; /* Grise la tab */
        cursor: not-allowed; /* Change le curseur */
    }
</style>


---

2. JavaScript pour gérer les interactions

Ajoutez un script qui vérifie l'état des champs titre1 et titre2 et ajuste les tabs en conséquence.

<script>
    $(document).ready(function () {
        // Identifiants des tabs et des champs
        const tab1Selector = '.coral-Tab[value="tab1"]';
        const tab2Selector = '.coral-Tab[value="tab2"]';
        const titre1Selector = 'input[name="./titre1"]';
        const titre2Selector = 'input[name="./titre2"]';

        // Fonction pour griser Tab 1 si Titre 2 est rempli
        function checkTab1State() {
            if ($(titre2Selector).val().trim() !== "") {
                $(tab1Selector).addClass("disabled-tab");
            } else {
                $(tab1Selector).removeClass("disabled-tab");
            }
        }

        // Fonction pour griser Tab 2 si Titre 1 est rempli
        function checkTab2State() {
            if ($(titre1Selector).val().trim() !== "") {
                $(tab2Selector).addClass("disabled-tab");
            } else {
                $(tab2Selector).removeClass("disabled-tab");
            }
        }

        // Attacher les événements sur les champs
        $(titre1Selector).on("input", function () {
            checkTab2State();
        });

        $(titre2Selector).on("input", function () {
            checkTab1State();
        });

        // Exécuter la vérification initiale au chargement
        checkTab1State();
        checkTab2State();
    });
</script>


---

3. Configuration dans le dialogue

Configurez vos tabs et champs dans le dialogue AEM avec les noms spécifiés (titre1 et titre2).

<tabs
    jcr:primaryType="nt:unstructured"
    sling:resourceType="granite/ui/components/coral/foundation/tabs">
    <items jcr:primaryType="nt:unstructured">
        <tab1
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container"
            jcr:title="Tab 1"
            value="tab1">
            <items jcr:primaryType="nt:unstructured">
                <titre1
                    jcr:primaryType="nt:unstructured"
                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                    fieldLabel="Titre 1"
                    name="./titre1" />
            </items>
        </tab1>
        <tab2
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container"
            jcr:title="Tab 2"
            value="tab2">
            <items jcr:primaryType="nt:unstructured">
                <titre2
                    jcr:primaryType="nt:unstructured"
                    sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                    fieldLabel="Titre 2"
                    name="./titre2" />
            </items>
        </tab2>
    </items>
</tabs>


---

Fonctionnement :

1. Tab 1 devient grisée si le champ titre2 de Tab 2 est rempli (et inversement pour Tab 2).


2. La vérification s'effectue à chaque modification du champ correspondant grâce à l'événement input.


3. Lors du chargement, le script vérifie déjà les valeurs pour ajuster l'état initial des tabs.




---

Résultat attendu :

Si l'utilisateur remplit le champ titre2 dans Tab 2, Tab 1 devient grisée et inaccessible.

Si l'utilisateur remplit le champ titre1 dans Tab 1, Tab 2 devient grisée et inaccessible.


