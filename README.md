﻿# bank-config-service

Pour griser une tab dans un dialogue AEM basé sur Granite UI et afficher un message si un utilisateur essaie d'y accéder, vous pouvez utiliser un mélange de CSS, JavaScript et Granite UI Listener.

Voici comment procéder :


---

1. Désactiver la Tab avec du CSS

Ajoutez une classe CSS pour griser la tab et désactiver son clic.

<style>
    .disabled-tab {
        pointer-events: none; /* Désactive les clics */
        opacity: 0.5; /* Grise la tab */
    }
</style>


---

2. Détecter et empêcher l'accès avec JavaScript

Ajoutez un listener Granite pour afficher un message si l'utilisateur essaie d'accéder à une tab désactivée.

<script>
    $(document).ready(function () {
        // Identifier les tabs à désactiver
        const tabsToDisable = ["tabId1", "tabId2"]; // Remplacez avec les IDs de vos tabs

        // Griser les tabs désactivées
        tabsToDisable.forEach(tabId => {
            $(`[data-id="${tabId}"]`).addClass("disabled-tab");
        });

        // Empêcher l'accès et afficher un message
        $(".coral-Tab").on("click", function (e) {
            const tabId = $(this).data("id");
            if (tabsToDisable.includes(tabId)) {
                e.preventDefault();
                Coral.commons.ready(this, () => {
                    Coral.Dialog.alert({
                        title: "Accès restreint",
                        content: "Cette section est désactivée et ne peut pas être modifiée.",
                        variant: "warning"
                    });
                });
            }
        });
    });
</script>


---

3. Configuration dans le dialogue AEM

Assurez-vous que chaque tab a un data-id unique. Par exemple :

<tabs
    jcr:primaryType="nt:unstructured"
    sling:resourceType="granite/ui/components/coral/foundation/tabs">
    <items jcr:primaryType="nt:unstructured">
        <tab1
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container"
            jcr:title="Tab 1"
            data-id="tabId1">
            <!-- Contenu de la tab -->
        </tab1>
        <tab2
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container"
            jcr:title="Tab 2"
            data-id="tabId2">
            <!-- Contenu de la tab -->
        </tab2>
    </items>
</tabs>


---

Résultat attendu

1. Les tabs avec data-id spécifiés dans tabsToDisable seront grisées et désactivées.


2. Un message d'alerte apparaîtra si l'utilisateur essaie de cliquer dessus.



Cette solution est non destructive, car elle n'altère pas les données, mais empêche l'accès via l'interface utilisateur.
