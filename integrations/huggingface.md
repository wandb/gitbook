---
description: >-
  W&B integration with the awesome NLP library Hugging Face, which has
  pre-trained models, scripts, and datasets
---

# Hugging Face

[Hugging Face Transformers](https://huggingface.co/transformers/) fournit des architectures à vocation générale pour la Compréhension du Langage Naturel \(NLU\) et la Génération Automatique de Textes \(NLG\) avec des modèles pré-entraînés dans plus de 100 langues et une profonde interopérabilité entre TensorFlow 2.0 et PyTorch. ​

Pour automatiquement obtenir l’enregistrement des entraînements, installez tout simplement la librairie et connectez-vous :

```text
pip install wandb
wandb login
```

Le `Trainer` ou `TFTrainer` enregistrera automatiquement les pertes, les mesures d’évaluation, la topologie du modèle et les dégradés.

Une configuration avancée est possible en passant par les [variables d’environnement wandb](https://docs.wandb.com/library/environment-variables).

Des variables supplémentaires sont disponibles avec transformers :

<table>
  <thead>
    <tr>
      <th style="text-align:left">Variables d&#x2019;environnement</th>
      <th style="text-align:left">Options</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">WANDB_WATCH</td>
      <td style="text-align:left">
        <ul>
          <li><b>gradients</b> (par d&#xE9;faut): Enregistre les histogrammes des d&#xE9;grad&#xE9;s</li>
          <li><b>all</b>: Enregistre les histogrammes de d&#xE9;grad&#xE9;s et les param&#xE8;tres</li>
          <li><b>false</b>: Aucun enregistrement de d&#xE9;grad&#xE9; ou de param&#xE8;tre</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left"><b>bool&#xE9;en : </b>R&#xE9;gler sur <b>true</b> pour totalement d&#xE9;sactiver
        l&#x2019;enregistrement</td>
    </tr>
  </tbody>
</table>

### Exemples

Nous avons créé quelques exemples pour que vous puissiez voir comment l’intégration fonctionne :

*  [Essayer dans colab ](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?usp=sharing): Un exemple simple de notebook pour bien commencer
* [Un guide pas-à-pas ](https://app.wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU): retracez les performances de votre modèle Hugging Face
*  [La taille du modèle est-elle importante ?](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-A-comparison-of-BERT-and-DistilBERT--VmlldzoxMDUxNzU) Une comparaison de BERT et DistilBERT

###  Retours

Nous serons ravis d’entendre vos retours et de pouvoir améliorer cette intégration. [Contactez-nous](https://docs.wandb.ai/company/getting-help) si vous avez des questions ou des suggestions à nous faire parvenir.

### **Visualisation des résultats**

Une fois que vous avez enregistré les résultats de votre entraînement, vous pouvez les explorez de façon dynamique dans le tableau de bord de W&B. Celui-ci vous permet d’avoir une vue d’ensemble sur desdouzaines d’expériences, de zoomer sur des résultats intéressants, et de visualiser des données hautement dimensionnelles.

![](../.gitbook/assets/hf-gif-15%20%282%29%20%282%29%20%283%29%20%283%29%20%283%29%20%281%29%20%281%29%20%281%29%20%281%29%20%284%29.gif)

Voici un exemple de comparaison pour [BERT vs DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU) – il est facile de voir comment des architectures différentes affectent la précision d’évaluation à travers l’entraînement, grâce aux visuels des graphiques linéaires automatiquement générés.

![](../.gitbook/assets/gif-for-comparing-bert.gif)

