---
description: Mesures automatiquement enregistrées par wandb
---

# System Metrics

`wandb` enregistre automatiquement les mesures système toutes les deux secondes, et en fait une moyenne sur une période de 30 secondes. Ces mesures comprennent :

*      **L’utilisation CPU**
* **L’utilisation mémoire système**
* **L’utilisation IOPS disque**
* **Le trafic réseau \(bytes envoyées et reçues\)**
* **L’utilisation GPU**
* **La température GPU**
* **Le temps passé en accès mémoire GPU \(en pourcentage de l’échantillon de temps\)·       La mémoire GPU allouée**

Les mesures GPU sont collectées sur une base par appareil en utilisant [nvidia-ml-py3](https://github.com/nicolargo/nvidia-ml-py3/blob/master/pynvml.py). Pour plus d’informations sur l’interprétation de ces mesures et l’optimisation des performances de votre modèle, consultez [cet article de blog utile de Lambda Labs](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/).

