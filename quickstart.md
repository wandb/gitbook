---
description: >-
  Instrumentez facilement un script pour voir nos fonctionnalités de suivi et de
  visualisation d’expérience sur votre propre projet.
---

# Quickstart

Initiez l’enregistrement de vos expériences d’apprentissage automatique en trois étapes rapides.

## 1. **Installer la bibliothèque**

Installez notre bibliothèque dans un environnement qui utilise Python 3.

```bash
pip install wandb
```

{% hint style="info" %}
Si vous entraînez des modèles dans un environnement automatisé où il n’est pas pratique d’avoir des commandes shell, comme le CloudML de Google, n’hésitez pas à consulter nos [variables d’environnement](https://docs.wandb.ai/library/environment-variables).
{% endhint %}

## 2. **Créer un compte**

Créez un compte gratuitement en vous inscrivant depuis votre shell ou sur notre [page d’inscription](https://app.wandb.ai/login?signup=true).

```bash
wandb login
```

## 3.  Modifier votre script d’entraînement

Ajoutez quelques lignes à votre script pour enregistrer des hyperparamètres et des métriques.

{% hint style="info" %}
Weights and Biases est agnostique en framework, mais si vous utilisez un framework d’apprentissage automatique courant, vous pourrez trouver des exemples relatifs à ce framework pour débuter encore plus facilement. Nous avons préparé des exemples spécifiques pour l’intégration de [Keras](file:///C:/integrations/keras), [TensorFlow](file:///C:/integrations/tensorflow), [PyTorch](file:///C:/integrations/pytorch), [Fast.ai](file:///C:/integrations/fastai), [Scikit](file:///C:/integrations/scikit), [XGBoost](file:///C:/integrations/xgboost), [Catalyst](file:///C:/integrations/catalyst).
{% endhint %}

### Initialiser W&B

Initialisez `wandb` au début de votre script, avant de commencer l’enregistrement. Certaines intégrations, comme notre intégration [Hugging Face](https://docs.wandb.ai/integrations/huggingface), comprennent déjà wandb.init\(\) en interne.

```python
# Inside my model training code
import wandb
wandb.init(project="my-project")
```

Nous créons automatiquement le projet pour vous s’il n’existe pas. Les exécutions du script d’entraînement susmentionné se synchroniseront à un projet nommé « my-project ». Consultez la documentation [wandb.init](file:///C:/library/init)pour explorer d’autres options d’initialisation.

## Déclarer des Hyperparamètres

Il est facile d’enregistrer des hyperparamètres avec l’objet [wandb.config](file:///C:/library/config).

```python
wandb.config.dropout = 0.2
wandb.config.hidden_layer_size = 128
```

### **Enregistrer des métriques**

Enregistrez des métriques comme ceux de vos pertes ou votre précision pendant que votre modèle s’entraîne \(dans de nombreux cas, nous en fournissons par défaut pour chaque framework\). Enregistrez des sorties ou des résultats plus complexes comme des histogrammes, des graphiques ou des images avec [wandb.log](file:///C:/library/log).

```python
def my_train_loop():
    for epoch in range(10):
        loss = 0 # change as appropriate :)
        wandb.log({'epoch': epoch, 'loss': loss})
```

### **Sauvegarder des fichiers**

 Tout ce qui est sauvegardé dans le répertoire `wandb.run.dir` sera téléchargé sur W&B et sauvegardé avec votre essai lorsqu’il se termine. C’est particulièrement pratique pour littéralement sauvegarder les poids \(weights\) et les biais \(biases\) de votre modèle :

```python
# by default, this will save to a new subfolder for files associated
# with your run, created in wandb.run.dir (which is ./wandb by default)
wandb.save("mymodel.h5")

# you can pass the full path to the Keras model API
model.save(os.path.join(wandb.run.dir, "mymodel.h5"))
```

Parfait ! Maintenant, exécutez votre script normalement et nous synchroniserons les enregistrements en tâche de fond. Vos résultats, métriques et fichiers seront synchronisés au cloud, avec l’enregistrement de votre état git, si vous utilisez un référentiel git \(git repo\).

{% hint style="info" %}
Si vous réalisez des tests et souhaitez désactiver la synchronisation wandb, configurez la [variable d’environnement](https://docs.wandb.ai/library/environment-variables) WANDB\_MODE=dryrun
{% endhint %}

### Étapes suivantes

Maintenant que l’instrumentation est en place, voici un bref aperçu de fonctionnalités sympas :

1.  **Page de projet** : comparez de nombreuses expériences grâce au tableau de bord du projet. À chaque fois que vous lancez un essai de modèle dans un projet, une nouvelle ligne apparaît sur les graphiques et dans le tableau. Cliquez sur l’icône du tableau dans le panneau latéral gauche pour augmenter la taille du tableau et visualiser tous vos hyperparamètres et vos métriques. Créez plusieurs projets pour organisez vos essais, et utilisez le tableau pour ajouter des étiquettes et des notes à vos essais.
2. **Visualisation personnalisée** : ajoutez des graphiques de coordonnées parallèles, des diagrammes de dispersion et d'autres visualisations avancées pour explorer vos résultats.
3. [**Rapports**](https://docs.wandb.ai/reports) ****: ajoutez un panneau Markdown pour décrire les résultats de vos recherches à côté de vosgraphiques animés et vos tableaux que vous obtenez en direct. Avec les rapports, il est facile de partager votre projet en une seule image pour vos collaborateurs, votre professeur ou votre patron !
4. ​[**Intégrations**](https://docs.wandb.ai/integrations) ****: nous avons des intégrations spéciales pour les frameworks les plus utilisés comme PyTorch, Keras et XGBoost.
5. **Présentations de recherches** : vous souhaitez partager vos recherches ? Nous travaillons en permanence sur des articles de blog pour mettre en avant le travail incroyable de notre communauté. Envoyez-nous un message à l’adresse contact@wandb.com.

###  ****[**Contactez-nous si vous avez des questions**](https://docs.wandb.ai/company/getting-help)**​**

###  [**Voir l’étude de cas d’OpenAI**](https://bit.ly/wandb-learning-dexterity)[ →](https://bit.ly/wandb-learning-dexterity)

![](.gitbook/assets/image%20%2891%29.png)

