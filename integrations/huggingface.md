---
description: >-
  W&B integration with the awesome NLP library Hugging Face, which has
  pre-trained models, scripts, and datasets
---

# Hugging Face

[Los Transformadores de Hugging Face](https://huggingface.co/transformers/) proveen arquitecturas de propósito general para el Entendimiento del Lenguaje Natural \(NLU\) y la Generación del Lenguaje Natural \(NLG\) con modelos pre-entrenados en más de 100 idiomas, y con una profunda interoperabilidad entre TensorFlow 2.0 y PyTorch.

Para hacer que el entrenamiento sea registrado automáticamente, solo instala la biblioteca e inicia sesión:

```text
pip install wandb
wandb login
```

`Trainer` o `TFTrainer` registrarán automáticamente las pérdidas, las métricas de la evaluación, la topología del modelo y los gradientes.

 La configuración avanzada es posible a través de las [variables de entorno de wandb](https://docs.wandb.com/library/environment-variables).

Hay variables adicionales disponibles con los transformadores:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Variables de Entorno</th>
      <th style="text-align:left">Opciones</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">WANDB_WATCH</td>
      <td style="text-align:left">
        <ul>
          <li><b>gradients (por defecto,):</b> Registra histogramas de los gradientes</li>
          <li><b>all:</b> Registra histogramas de los gradientes y de los par&#xE1;metros</li>
          <li><b>false</b>: No registra ni a los gradientes ni a los par&#xE1;metros</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">WANDB_DISABLED</td>
      <td style="text-align:left"><b>booleano: </b>Establ&#xE9;celo a true para deshabilitar el registro
        por completo</td>
    </tr>
  </tbody>
</table>

### Ejemplos

Hemos creado algunos ejemplos para que veas cómo funciona la integración:

* [Ejecuta en colab](https://colab.research.google.com/drive/1NEiqNPhiouu2pPwDAVeFoN4-vTYMz9F8?usp=sharing): Una notebook de ejemplo simple para que des los primeros pasos
* [Una guía paso a paso](https://app.wandb.ai/jxmorris12/huggingface-demo/reports/A-Step-by-Step-Guide-to-Tracking-Hugging-Face-Model-Performance--VmlldzoxMDE2MTU): haz el seguimiento del desempeño del modelo de Hugging Face
*  [¿Importa el tamaño del modelo?](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-A-comparison-of-BERT-and-DistilBERT--VmlldzoxMDUxNzU) Una comparación de BERT y DistilBERT

### Comentarios

Nos encantaría oír los comentarios y estamos entusiasmados por mejorar esta integración. [Comunícate con nosotros](https://docs.wandb.ai/company/getting-help) para hacer cualquier pregunta o plantear sugerencias.

## Visualizar Resultados

Explora tus resultados dinámicamente en el Tablero de Control de W&B. Es fácil buscar a través de docenas de experimentos, enfocarte en los resultados interesantes, y visualizar los datos multidimensionales.

![](../.gitbook/assets/hf-gif-15%20%282%29%20%282%29%20%283%29%20%283%29%20%283%29%20%281%29%20%281%29%20%281%29%20%281%29%20%284%29.gif)

Aquí hay un ejemplo que compara [BERT vs DistilBERT](https://app.wandb.ai/jack-morris/david-vs-goliath/reports/Does-model-size-matter%3F-Comparing-BERT-and-DistilBERT-using-Sweeps--VmlldzoxMDUxNzU) – es fácil ver cómo las diferentes arquitecturas afectan a la precisión de la evaluación durante todo el entrenamiento, a través de las visualizaciones de los diagramas de líneas automáticos.

![](../.gitbook/assets/gif-for-comparing-bert.gif)

