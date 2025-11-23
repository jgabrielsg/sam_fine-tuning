# Fine-Tuning do Segment Anything Model (SAM) para Segmentação de Tumores Cerebrais

Este repositório contém o trabalho de implementação de um fine-tuning a partir do SAM (Segment Anything Model) da Meta AI, adaptado para a tarefa específica de segmentação de tumores em exames de ressonância magnética (MRI).

### Autores
- João Gabriel
- Guilherme Buss
- Vinícius Nascimento

### Objetivo

O SAM é um modelo de fundação (Foundation Model) treinado em imagens genéricas. Este projeto visa adaptar esse conhecimento generalista para o domínio médico, especificamente para identificar e segmentar três tipos de tumores cerebrais (Glioma, Meningioma e Pituitário), demonstrando que o fine-tuning de poucas camadas é suficiente para obter resultados precisos em tarefas especializadas.

### Como Fizemos

Nossa implementação foca em duas coias: maior eficiência computacional e adaptação de domínio. Fizemos:

- **Estratégia de Congelamento:** Para viabilizar o treino em GPUs limitadas (ex: Colab), congelamos os pesos do Image Encoder (ViT) e do Prompt Encoder. Realizamos o fine-tuning **apenas no Mask Decoder**, que é a parte leve e responsável pela geração final da máscara.
- **Adaptação de Resolução:** O SAM original opera em 1024x1024. Realizamos uma interpolação nos *Positional Embeddings* para permitir que o modelo aceite imagens de 256x256, acelerando o treinamento sem perder a capacidade espacial.
- **Prompt Engineering Automático:** Durante o treinamento, simulamos prompts de usuário convertendo os Ground Truth em *Bounding Boxes*, que é o prompt que modelo irá receber.
- **Função de Perda:** Utilizamos uma combinação de Dice Loss (focada na sobreposição da forma) e Binary Cross Entropy comum, como sugerido no trabalho.

### Detalhes da Implementação
- **Dataset:** [Brain Tumor Segmentation Dataset](https://www.kaggle.com/datasets/atikaakter11/brain-tumor-segmentation-dataset) (Kaggle).
- **Classes:** 1 (Glioma), 2 (Meningioma), 3 (Pituitário) (sem tumor não foi utilizada, não há prompts).
- **Modelo Base:** SAM ViT-B (Vision Transformer Base).

### Experimentos e Resultados

O modelo foi avaliado utilizando a métrica IoU, comparando a máscara predita pelo SAM com a máscara real do especialista.

**Resultados Principais:**
1. **Adaptação Rápida:** Com poucas épocas de treinamento apenas no decodificador, o modelo aprendeu a ignorar ruídos típicos de imagens médicas e focar na textura do tumor.
2. **Generalização:** O modelo demonstrou capacidade de segmentar corretamente os três tipos de tumores, lidando bem tanto com tipos diferentes de bordas em regiões diferentes do cérebro.
3. **Eficiência:** A redução de dimensionalidade para 256x256 permitiu iterações rápidas de treino mantendo a qualidade da segmentação.

---

### Recursos do Projeto

### Vídeo da Apresentação
Link: [VIDEO](https://www.youtube.com/watch?v=F_8l5bHjjgo)

---
