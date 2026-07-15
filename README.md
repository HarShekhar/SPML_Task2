# SPML Task 2 Submission

## Repository Structure
Task2_Submission/
├── Base_ML/
│   ├── Level_1_ResNet/      # ResNet from scratch on CIFAR-10 (89% accuracy)
│   ├── Level_2_LSTM/        # Custom LSTM on Jena Climate (MAE: 0.48°C)
├── Applied_ML/              # Healthcare Information Assistant (RAG system)

## Base ML
### Level 1 - ResNet
- Custom ResNet with 3 stages (32→64→128 channels)
- Skip connections with 1x1 conv projections for dimension mismatches
- Global Average Pooling before classifier
- 89% test accuracy on CIFAR-10

### Level 2 - LSTM  
- Fully hand-rolled LSTM cell (input/forget/output gates + candidate state)
- 2 stacked LSTM layers, hidden dim 64
- Jena Climate dataset, 72h input → 12h forecast
- MAE: 0.48°C, RMSE: 0.73°C

## Applied ML - Healthcare Information Assistant
- RAG pipeline: MedQuAD + WHO + CDC + NICE guidelines
- Embeddings: all-MiniLM-L6-v2, Vector store: FAISS
- Generation: Qwen2.5-3B-Instruct (4-bit quantized)
- Features: grounded generation, citations, confidence estimation, safety layer
- UI: Gradio

## How to Run
1. Open the respective Kaggle notebook
2. Attach the required dataset (MedQuAD for Applied ML, CIFAR-10/Jena Climate auto-downloads)
3. Run all cells in order

## References
- MedQuAD: https://www.kaggle.com/datasets/gpreda/medquad
- Jena Climate: https://www.kaggle.com/datasets/mnassrib/jena-climate
- WHO Hypertension: https://www.who.int/news-room/fact-sheets/detail/hypertension
- CDC: https://www.cdc.gov/high-blood-pressure/about/index.html
- NICE: https://www.nice.org.uk/guidance/ng136
"""

with open('/kaggle/working/Task2_Submission/README.md', 'w') as f:
    f.write(readme)
