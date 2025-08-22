# Detecção de Anomalias em Transações Financeiras

Este projeto implementa um sistema de detecção de fraudes e anomalias em transações financeiras, combinando modelos estatísticos, Autoencoder, KMeans e XGBoost, além de regras baseadas em comportamento transacional.

Este repositório contém apenas a documentação do projeto. O código-fonte está privado.

---

## Estrutura de Modelos

O projeto utiliza diversos modelos e artefatos, organizados na pasta `modelos/`:

| Arquivo | Descrição |
|---------|-----------|
| `scaler.pkl` | Escalonador dos dados numéricos. |
| `colunas_scaler.pkl` | Lista das colunas utilizadas no scaler. |
| `modelo_encoder.keras` | Modelo de codificação latente para o Autoencoder. |
| `modelo_autoencoder.keras` | Autoencoder usado para reconstrução e detecção de anomalias. |
| `kmeans_auto.pkl` | KMeans aplicado no espaço latente para detectar desvios de cluster. |
| `encoder_tipo_transacao.pkl` | Encoder para o tipo de transação. |
| `encoder_semana.pkl` | Encoder para o dia da semana. |
| `encoder_horario.pkl` | Encoder para a faixa horária. |
| `modelo_xgb.pkl` | Modelo XGBoost treinado para classificação de fraude. |

---

## Funcionalidades Principais

1. **Carregamento de modelos**  
   Função `carregar_modelos()` carrega todos os modelos e artefatos necessários.

2. **Cálculo de Erros e Distâncias**  
   - `calcular_erros_e_distancias(df, modelos)` calcula:
     - **Erro de reconstrução** usando Autoencoder.
     - **Distância ao cluster mais próximo** usando KMeans no espaço latente.
   - Permite identificar transações que não seguem o comportamento padrão.

3. **Detecção de Anomalias por Regras e Pesos**  
   - `detectar_anomalias(df, modelos)` cria regras como:
     - Valor da transação acima de 3 desvios padrão da média.
     - Transações na madrugada.
     - Alta frequência de transações.
     - Desvio do comportamento do cluster.
   - Combina regras em uma **pontuação de fraude** e gera `fraude_confirmada`.

4. **Motivos do alerta**  
   - `gerar_motivo_alerta(row)` identifica os critérios pelos quais uma transação foi marcada como suspeita, incluindo:
     - Erro de reconstrução alto, distância ao cluster alta, valor elevado, horário suspeito, frequência alta e desvio do cluster.

5. **Treinamento e Avaliação XGBoost**  
   - O modelo XGBoost (`XGBClassifier`) é treinado com oversampling (SMOTE) para lidar com desbalanceamento.
   - `avaliar_thresholds()` testa thresholds para otimizar F1-score e igualdade de oportunidade.
   - `validacao_temporal()` realiza validação por períodos de tempo para garantir generalização.

6. **Score contínuo e Faixa de risco**  
   - `gerar_score_continuo()` combina resultado do modelo e regras para gerar `score_final` e categorizar o risco em baixo, moderado e alto.

7. **Análise de falsos negativos**  
   - `analisar_falsos_negativos(df)` identifica transações críticas que não foram detectadas pelo modelo, mas apresentam alto erro de reconstrução ou distância ao cluster.

---

## Avaliação de Performance

Métricas calculadas:

| Métrica | Descrição |
|---------|-----------|
| F1-score | Balanceamento entre precisão e recall. |
| Recall por grupo | Avaliação da igualdade de oportunidade. |
| Falsos negativos | Fraudes não detectadas. |
| Falsos positivos | Alarmes falsos. |
| Acertos | Transações corretamente classificadas. |

---

## Tecnologias Utilizadas

- Python 3.x  
- Pandas, NumPy  
- TensorFlow / Keras (Autoencoder e Encoder)  
- scikit-learn (pré-processamento, métricas, KMeans)  
- imbalanced-learn (SMOTE)  
- XGBoost (classificação)  
- Matplotlib (visualização)  

---
