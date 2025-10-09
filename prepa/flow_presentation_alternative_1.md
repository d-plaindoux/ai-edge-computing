IA et Edge Computing : traitement du langage efficace, sobre et sans dépendance

1. Accroche : Le défi (5 minutes)
    - Imaginez, vous êtes mi 2023 et l'on vous demande :
        - de classifier des phrases (dans notre cas des titres de documents) 
        - d'extraire des entitées nommées, la liste des entitiées possibles étant inconnue à l'avance
        - de faire cela en moins < 1 seconde, sur un appareil avec peu de ressources (un PC portable)
        - et sans envoyer les données à un serveur distant tout doit être fait sur le EDGE
        - sans dépendances a des bibliothèques lourdes (tensorflow, pytorch etc.)
        Demo montrer un exemple.

     - Pourquoi est-ce un défi ?
        - Contraintes de ressources (CPU, mémoire)
        - Latence (temps réel)
        - Confidentialité des données (pas de transfert vers le cloud)
        - Variabilité des données (titres de documents très différents les uns des autres)

     - Promesse :
        - Nous allons voir comment relever ce défi en utilisant des techniques d'IA et d'Edge Computing.
        - Nous allons explorer des solutions innovantes pour traiter le langage de manière efficace et sobre, tout en respectant les contraintes imposées par l'Edge Computing.   
        - Cette présentation s'adresse à un public de niveau Intermédiaire. Nous n'allons pas réexpliquer les bases du Machine Learning, mais nous concentrer sur l'architecture et l'implémentation d'une solution concrète.

2. La boite a outils (10 minutes)
    - Le réflexe, le tentation actuelle voudrait que l'on se precipite vers les LLMs (GPT, Mistral, Gemini etc.) Cependant certaines de ces modèles sont disponbles soit uniquement via des API donc cela implique d'envoyer les données vers le cloud, d'autres sont disponibles en local mais ils sont très gourmands en ressources (GPU, RAM) et donc pas adaptés pour l'Edge.

   - Si l'on prend un peu de recul, on voit que un modèle comme BERT (2018) est déjà très performant pour le traitement du langage naturel (NLP) et peut être adapté pour des tâches spécifiques comme la classification de texte ou l'extraction d'entités nommées. De plus, il existe des versions allégées de BERT, comme DistilBERT ou TinyBERT, qui sont conçues pour être plus efficaces en termes de ressources.

   - mais pour le premier cas d'usage (classification de texte) on peut aller encore plus loin ecartant tout modèle d'IA et revenir a des méthodes basées sur la theorie de l'information. cf. papier https://aclanthology.org/2023.findings-acl.426.pdf (gzip + k-NN)

   - Evidement cette technique ne fonctionne que si l'on est en presence de texte qui utilisent un vocabulaire commun. En effet ce modèle ne permet pas de capturer les rélations sémantiques entre les mots. Par exemple, si l'on a un texte qui parle de "voiture" et un autre qui parle de "véhicule", le modèle ne comprendra pas que ces deux mots sont liés. Cependant, dans notre cas d'usage (classification de titres de documents), nous avons un vocabulaire relativement restreint et commun, ce qui rend cette approche viable pour une premiére version.

   - C'est important pour nous de commencer par une solution simple, efficace, avec un niveau de performance acceptable, avant de complexifier la solution si nécessaire. Cela à pour avantage de pouvoir itérer rapidement et surtôut d'integrer la solution dans un produit en production.

    - Pour le deuxieme cas d'usage (extraction d'entités nommées) on va utiliser un modèle de type BERT allégé (DistilBERT) que l'on va fine-tuner sur notre jeu de données spécifique. Cela nous permettra d'obtenir de bonnes performances tout en restant dans les contraintes de ressources imposées par l'Edge Computing.

3. Focus sur l'extraction d'entités nommées (20 minutes)
    - Gliner (https://arxiv.org/pdf/2311.08526)
        - Présentation du modèle Gliner, un modèle de type BERT allégé conçu pour l'extraction d'entités nommées.
        - Architecture du modèle : couches d'attention, couches feed-forward, etc.
        - Avantages du modèle : efficacité en termes de ressources, bonnes performances sur des tâches spécifiques.

    - Problèmes pour l'inférence sur le EDGE
        - code source pas vraiment production ready
        - quelques bugs
        - optimisations nécessaires
        - concepts mélangés (entrainement, inférence, évaluation) -> difficile de s'y retrouver
        - documentation à améliorer
        - disponibles uniquement en Python

    - Mais aussi beaucoup de points positifs
        - modèle pré-entraîné disponible
        - finetunning possible (et facile quand on à l'habitude)
        - export du modèle en ONNX (Open Neural Network Exchange) pour une inférence plus rapide et plus légère
        - communauté active
        - licence Apache 2.0

    - Notre architecture pour l'inference sur le EDGE
        - Modéle ONNX et runtime  d'inference en RUST.
        - Avantages 
            - support multi-plateforme, optimisations pour différentes architectures matérielles, facilité d'intégration dans des applications existantes.
            - performance, sécurité, gestion efficace de la mémoire, écosystème en croissance pour le machine learning (tch-rs, onnxruntime-rs etc.)
        - Déploiement facile sur différentes plateformes (Linux, Windows, MacOS)
        - Notre plateforme de déploiement automatique sur le EDGE (demo )
        - Démonstration de l'inférence sur un appareil Edge (PC portable) avec des exemples de titres de documents.

4. Limites (5 minutes)
    - Limites de la solution actuelle
        - Performances : bien que le modèle soit efficace, il peut ne pas atteindre les performances des modèles plus lourds. Cependant, il offre un bon compromis entre performance et efficacité.
        - Scalabilité : taille de contexte limitée (512 tokens pour BERT), ce qui peut poser problème pour des textes plus longs. Nous faisons du chunking plus batching pour contourner cette limite. Mais cela augment le temps de traitement.

    - Perspectives d'amélioration
        - Tester des LLM plus avancès comme 
            - BitNet 1.5 bits optimisé pour l'Edge.
            - Mistral Edge (3B / 7B / 8B) optimisé pour l'Edge.
            - autre SLM optimisés pour l'Edge.


5. Conclusion (5 minutes)
  Résumez les 3 messages clés à retenir :
  - Pour des tâches précises, les LLM ne sont pas toujours la solution.
  - Les modèles comme BERT sont des spécialistes redoutables et bien plus légers.
  - Le duo ONNX + Rust est une recette gagnante pour déployer de l'IA performante et multiplateforme en local.