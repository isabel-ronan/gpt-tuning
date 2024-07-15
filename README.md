# GPT Tuning

- This repository houses the code to fine-tune a GPT-3.5 Turbo model on clinical dialogue. 
- The fine-tuned GPT model is trained using the `aci-bench` dataset which is available in Microsoft's [clinical_visit_note_summarization_corpus](https://github.com/microsoft/clinical_visit_note_summarization_corpus) along with the [OpenAI API](https://openai.com/index/openai-api/).
- Fine-tuned results will be compared to standard model outputs and human outputs; these results will be reported for various fine-tuned models.

## Methodology
1. Format the `aci-bench` dataset for OpenAI fine-tuning. ✅
2. Validate data using [OpenAI's validation script](https://cookbook.openai.com/examples/chat_finetuning_data_prep). ✅
3. Train a "default" model using the `gpt-3.5-turbo-012` model (as recommended by OpenAI in their [docs](https://platform.openai.com/docs/guides/fine-tuning#:~:text=gpt%2D3.5%2Dturbo%2D0125%20(recommended))). ✅
4. Train using different batch sizes, learning rates, and epochs. ✅
5. Assess quality of models. 🔄


## Different Hyperparameters Tested - Grid Search Strategy
| Model Suffix    | Trained Tokens |Learning Rate Multiplier| Epochs    | Batch Size | Training Loss | Validation Loss | Model Name |
| ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |
| default-aci-bench | 472,395 | 2 | 3 | 1 | 0.6734 | 0.8788 | ft:gpt-3.5-turbo-0125:personal:default-aci-bench:9lDAbkNy |
| id0 | 472,395 | 0.5 | 3 | 1 | 0.8642 | 0.9285 | ft:gpt-3.5-turbo-0125:personal:id0-0-5-3-1:9lHCKWt6 |
| id1 | 472,395 | 0.5 | 3 | 32 | 1.1142 | 0.9966 | ft:gpt-3.5-turbo-0125:personal:id1-0-5-3-32:9lH86WrE |
| id2 | 472,395 | 0.5 | 3 | 67 | 1.2398 | 1.1496 | ft:gpt-3.5-turbo-0125:personal:id2-0-5-3-67:9lH8UvK4 |
| id3 | 1,574,650 | 0.5 | 10 | 1 | 0.7422 | 0.9144 | ft:gpt-3.5-turbo-0125:personal:id3-0-5-10-1:9lHvfLG8 |
| id4 | 1,574,650 | 0.5 | 10 | 32 | 0.8974 | 0.9212 | ft:gpt-3.5-turbo-0125:personal:id4-0-5-10-32:9lHdGty4 |
| id5 | 1,574,650 | 0.5 | 10 | 67 | 0.9426 | 0.9473 | ft:gpt-3.5-turbo-0125:personal:id5-0-5-10-67:9lHc7RWd |
| id6 | 472,395 | 1 | 3 | 1 | 0.7511 | 0.8946 | ft:gpt-3.5-turbo-0125:personal:id6-1-3-1:9lInZ0Yi |
| id7 | 472,395 | 1 | 3 | 32 | 1.0497 | 0.9494 | ft:gpt-3.5-turbo-0125:personal:id7-1-3-32:9lIiZDyS |
| id8 | 472,395 | 1 | 3 | 67 | 1.0948 | 1.0236 | ft:gpt-3.5-turbo-0125:personal:id8-1-3-67:9lIhXPXC |
| id9 | 1,574,650 | 1 | 10 | 1 | 0.5437 | 0.9825 | ft:gpt-3.5-turbo-0125:personal:id9-1-10-1:9lJxwuWa |
| id10 | 1,574,650 | 1 | 10 | 32 | 0.8195 | 0.8851 | ft:gpt-3.5-turbo-0125:personal:id10-1-10-32:9lJkMCw1 |
| id11 | 1,574,650 | 1 | 10 | 67 | 0.8833 | 0.9081 | ft:gpt-3.5-turbo-0125:personal:id11-1-10-67:9lJkGqJ5 |
| id12 | 472,395 | 2 | 3 | 1 | 0.6681 | 0.8778 |ft:gpt-3.5-turbo-0125:personal:id12-2-3-1:9lJz2sxA |
| id13 | 472,395 | 2 | 3 | 32 | 1.0097 | 0.9205 | ft:gpt-3.5-turbo-0125:personal:id13-2-3-32:9lJt3emA |
| id14 | 472,395 | 2 | 3 | 67 | 1.0013 | 0.9812 | ft:gpt-3.5-turbo-0125:personal:id14-2-3-67:9lK2HhyF|
| id15 | 1,574,650 | 2 | 10 | 1 | 0.2662 | 1.1277 | ft:gpt-3.5-turbo-0125:personal:id15-2-10-1:9lKVnn72 |
| id16 | 1,574,650 | 2 | 10 | 32 | 0.7011 | 0.8623 | ft:gpt-3.5-turbo-0125:personal:id16-2-10-32:9lKCyb7z |
| id17 | 1,574,650 | 2 | 10 | 67 | 0.7983 | 0.8676 | ft:gpt-3.5-turbo-0125:personal:id17-2-10-67:9lKF9kn8 |

- Learning Rates Tested = 0.5, 1, 2
- Epochs Tested = 3, 10
- Batch Sizes Tested = 1, 32, 67 (batch gradient descent)
