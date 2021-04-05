# Scatter Plot

Utilisez le nuage de points \(diagramme de dispersion\) pour comparer plusieurs essais et visualiser les performances de vos expériences. Nous avons ajouté quelques fonctionnalités personnalisables :

1. **Tracer une ligne le long du min, du max, et de la moyenne**
2. **Personnaliser les info-bulles de métadonnées**
3. **Contrôler les couleurs de points**
4. **Paramétrer les plages d’axes**
5. **Changer les axes en échelles logarithmiques**

Voici un exemple de précision de validation de modèles différents sur plusieurs semaines d’expériences. L’info-bulle est personnalisée pour inclure la taille de lot \(batch size\) et le dropout, ainsi que les valeurs sur les axes. Il y a aussi une ligne qui trace la moyenne courante de la précision de validation

[Voir un exemple en direct →](https://app.wandb.ai/l2k2/l2k/reports?view=carey%2FScatter%20Plot)

![](https://paper-attachments.dropbox.com/s_9D642C56E99751C2C061E55EAAB63359266180D2F6A31D97691B25896D2271FC_1579031258748_image.png)

##  Questions fréquentes

###  Est-il possible de tracer le maximum d’une mesure plutôt que de tracer chaque étape ?

La meilleure manière de faire ceci est de créer un Nuage de Points de cette mesure, de se rendre dans le menu Éditer, et de sélectionner Annotations. De là, vous pouvez tracer les valeurs courantes maximum.

