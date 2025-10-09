IA et Edge Computing : traitement du langage efficace, sobre et sans dépendance

1) Motivation de la session
    - Les raisons de cette proposition
    - Les raisons de vouloir recourir à des approches légères et sobres en ressources

2) Présentation des approches
    - l'IA != LLM
    - Historique rapide (IA symbolique et IA connexionniste)
    - Les LLM rentrent dans la categorie connexionniste mais ne sont pas la seule approche
    - Les avantages et inconvénients des LLM
    - LLMs une question de taille

3) Présentation des alternatives
    - Systèmes experts
    - Pas de LLM du tout (exemple atypique de classification par compression gzip + k-NN)
    - Présentation des modèles BERT et dérivés
    - SLM (Small Language Models)

4) REX
    - Les problème : NER zero-shot, données sensibles, infrastructure sans GPU
    - Explications : BERT + classification de tokens
    - Execution : ONNX Runtime + Rust
    - Distribution : agents locaux, autonomes et légers

5) Limites
    - Cas où les LLM sont plus efficaces
    - Désambiguïsation
    - Raisonnement
    - Manipulation de contextes complexes

6) Conclusion
    - Résumé des points clés
    - Questions / Réponses 

