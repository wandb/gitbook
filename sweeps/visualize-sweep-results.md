# Visualize Sweep Results

### Coordonnées parallèles

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695138341_image.png)

Les coordonnées parallèles tracent les valeurs d’hyperparamètres en fonction des mesures de modèles. Elles sont utiles pour se concentrer sur des combinaisons d’ \*\*\_ hyperparamètres qui ont mené aux meilleures performances du modèle.

## Graphique d’importance d’hyperparamètre

![](https://paper-attachments.dropbox.com/s_194708415DEC35F74A7691FF6810D3B14703D1EFE1672ED29000BA98171242A5_1578695757573_image.png)

 Ce panneau fait ressortir lesquels de vos hyperparamètres ont été de bons prédicteurs, et les corrélations importantes qu’ils ont avec les valeurs désirables de vos mesures.

**Correlation** est la corrélation linéaire entre l’hyperparamètre et la mesure choisie \(dans ce cas, val\_loss\). Donc, une haute corrélation signifie que lorsque cet hyperparamètre a une grande valeur, la mesure a aussi de grandes valeurs, et vice versa. La corrélation est une très bonne mesure à regarder, mais elle ne peut pas capturer les interactions de second plant entre les inputs, et ça peut devenir compliqué de comparer les inputs avec des plages très différentes.

C’est pourquoi nous calculons également une mesure d’**importance**, où nous entraînons une forêt aléatoire avec des hyperparamètres comme inputs, et la mesure comme output cible, et nous rapportons les valeurs d’importance de fonctionnalité pour la forêt aléatoire.

