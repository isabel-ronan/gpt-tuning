# GPT Tuning

- This repository houses the code to fine-tune a GPT-3.5 Turbo model on clinical dialogue. 
- The fine-tuned GPT model is trained using the `aci-bench` dataset which is available in Microsoft's [clinical_visit_note_summarization_corpus](https://github.com/microsoft/clinical_visit_note_summarization_corpus) along with the [OpenAI API](https://openai.com/index/openai-api/).
- Fine-tuned outputs will be compared to human outputs; these results will be reported for various fine-tuned models.

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

## Quality Assessment
| id                 | rouge1   | rouge2   | rougeL   | rougeLsum | bertScore | average   |
|--------------------|----------|----------|----------|-----------|-----------|-----------|
| no-fine-tuning| 0.47426083   | 0.176298955   | 0.249168809   | 0.456341087 | 0.632515335 | 0.397717003 |
| default-aci-bench| 0.547838677 | 0.233264252   | 0.28864903   | 0.532429578 | 0.683181063 | 0.45707252   |
| id0 | 0.461465998   | 0.161439656   | 0.222589831 | 0.447362259 | 0.645534579 | 0.387678465 |
| id1 | 0.488685346 | 0.1894922 | 0.254346586 | 0.473521086 | 0.646550941 | 0.410519232 |
| id2 | 0.483401849 | 0.185078423 | 0.25226153 | 0.465779684 | 0.64290905 | 0.405886107 |
| id3 | 0.543767716 | 0.229418259 | 0.288665606 | 0.528047338 | 0.683086141 | 0.454597012 |
| id4 | 0.457772558 | 0.151501786 | 0.214275959 | 0.443538071 | 0.637459373 | 0.38090955 |
| id5 | 0.425489118 | 0.131339777 | 0.194310695 | 0.411636878 | 0.627833875 | 0.358122069 |
| id6 | 0.518722606 | 0.201024527 | 0.263795354 | 0.50359768 | 0.673006248 | 0.432029283 |
| id7 | 0.431250634 | 0.139421432 | 0.202982031 | 0.416744121 | 0.629479472 | 0.363975538 |
| id8 | 0.484294375 | 0.183704202 | 0.250879846 | 0.468147533 | 0.64547507 | 0.406500205 |
| id9 | 0.579604761 | 0.274227518 | 0.331028468 | 0.56517976 | 0.701949692 | 0.49039804 |
| id10 | 0.504062534 | 0.183951389 | 0.241417895 | 0.489547927 | 0.659888776 | 0.415773704 |
| id11 | 0.461714149 | 0.156300151 | 0.219464038 | 0.446361916 | 0.642121379 | 0.385192327 |
| id12 | 0.552500706 | 0.236540997 | 0.296557143 | 0.538424824 | 0.68781325 | 0.462367384 |
| id13 | 0.401570797 | 0.121551783 | 0.180547696 | 0.388084168 | 0.617024422 | 0.341755773 |
| id14 | 0.429907316 | 0.139586073 | 0.203203591 | 0.414540341 | 0.63207709 | 0.363862882 |
| id15 | 0.601780641 | 0.299587614 | 0.354806801 | 0.587760463 | 0.715226936 | 0.511832491 |
| id16 | 0.548802961 | 0.22514115 | 0.284379352 | 0.532321942 | 0.682127571 | 0.454554595 |
| id17 | 0.500528754 | 0.182462544 | 0.23783652 | 0.484936127 | 0.654691569 | 0.412091103 |

