# Réflexions — Smart Mobility Le Mans
## 1. Question métier : facile ou difficile ?

Avant de commencer, je pensais qu'il serait assez facile de décrire l'offre planifiée et l'état du réseau à un instant donné. En revanche, je m'attendais à ce qu'il soit difficile de dire où le transport répond réellement aux besoins : il n'y a pas de données ouvertes de fréquentation, et un instantané ne suffit pas pour identifier des tendances. Cette prédiction est à ajuster si mon attente initiale était différente.

## 2. Pourquoi ne pas actualiser le GTFS statique toutes les 15 minutes ?

Le GTFS statique décrit les horaires et les arrêts planifiés. Son contenu ne change pas au rythme des véhicules : le télécharger toutes les 15 minutes répéterait surtout le même travail et produirait peu de nouvelles informations. Une actualisation mensuelle est plus proportionnée à la cadence de mise à jour de la source.

## 3. Taille de l'historique : arrêts ou lignes ?

Sur 12 semaines, 15 minutes par instantané représentent 96 instantanés par jour pendant 84 jours. Avec plus de 5 000 mises à jour d'arrêts à chaque instantané, cela dépasserait 40 millions de lignes. En enregistrant plutôt 27 à 40 lignes par instantané, une par ligne de transport, on obtiendrait environ 220 000 à 323 000 lignes. Ce choix allège fortement l'historique, mais rend plus difficile l'identification des arrêts touchés de façon répétée par des retards.

## 4. Une autre façon d'encoder les retards

Le graphique actuel rend les retards précoces verts et les retards importants rouges. Je proposerais une échelle divergente autour d'une zone de ponctualité : vert près de l'heure prévue, ambre pour les départs très précoces et orange à rouge pour les retards tardifs. Les seuils devraient être discutés avec l'exploitant. Cette représentation éviterait de traiter tout départ en avance comme une bonne performance et attirerait aussi l'attention sur les voyageurs qui risquent de manquer un véhicule parti trop tôt.

## 5. Pourquoi les cadences statique et temps réel sont-elles différentes ?

Le GTFS statique change à une cadence relativement lente ; le relire toutes les 15 minutes ne rendrait pas les horaires plus exacts. À l'inverse, des instantanés temps réel mensuels ne montreraient qu'une situation isolée et manqueraient les perturbations ainsi que leur évolution. La fréquence de collecte doit donc suivre la vitesse à laquelle chaque source et le phénomène observé changent.

## 6. Vérification d'un retard par rapport au terrain

Dans le premier instantané collecté, la ligne 5 affiche un retard moyen de 145,2 secondes sur 16 trajets observés et un retard maximal de 735 secondes. C'est un signal à examiner, pas encore une explication. Je n'ai pas vérifié cet épisode auprès de SETRAM ni identifié un événement local correspondant ; je ne peux donc pas conclure s'il s'agissait d'une perturbation réelle ou d'un artefact de données.

## 7. Population et demande de transport

Une commune peu peuplée peut accueillir une zone d'emplois ou de commerces qui attire chaque jour des salariés et des visiteurs venant d'autres communes. Sa population résidente sous-estimerait alors les usagers potentiels du réseau. C'est un exemple plausible à confronter au contexte local, pas une demande mesurée par ce jeu de données.

## 8. Indicateur choisi et commune où il peut induire en erreur

J'ai calculé les passages programmés aux arrêts par jour pour 1 000 habitants : somme des `daily_trip_count` des arrêts affectés à la commune, divisée par sa population, puis multipliée par 1 000. La-Chapelle-Saint-Aubin atteint environ 1 010 passages pour 1 000 habitants, contre environ 154 pour Le Mans. Ce résultat mérite vérification : le faible dénominateur et des arrêts desservant des personnes de passage peuvent faire paraître la commune exceptionnellement bien pourvue. L'indicateur mesure une offre planifiée rapportée aux résidents, pas le nombre de voyageurs ni l'accessibilité effective des habitants.

## 9. Réduire le volume d'un historique au niveau des arrêts

Je pourrais calculer chaque jour, pour chaque arrêt, le nombre d'observations, le retard moyen, un percentile élevé des retards et le nombre de passages supprimés, puis n'archiver qu'une ligne par arrêt et par jour. Cela remplacerait jusqu'à 96 lignes par arrêt et par jour par une seule ligne quotidienne. Il faudrait conserver la période d'agrégation et le nombre d'observations, et accepter que les pointes très brèves soient moins visibles. Une sélection complémentaire des arrêts présentant des retards répétés permettrait de limiter encore la taille du fichier.

## 10. Ce qui est déjà mesurable et ce qui demande du recul

Les horaires statiques permettent déjà de décrire une offre planifiée par arrêt et commune, et le collecteur donne un état du réseau à un instant précis. Les choix de jour représentatif, d'affectation à la commune la plus proche, de population comme proxy et d'encodage des KPI demandent un jugement explicite. Avec un seul instantané distinct dans l'historique actuel, je ne peux pas établir de récurrence ni faire une prévision fiable : il faudra plusieurs semaines de collecte régulière pour commencer à étudier des profils récurrents.

## 11. Retour sur la prédiction initiale

En reprenant l'hypothèse proposée en réflexion 1, j'avais anticipé que l'offre planifiée et l'état courant seraient plus faciles à décrire que les besoins réels et les tendances. Les résultats confirment cette distinction : les workflows ont produit les fichiers de référence et un instantané exploitable, mais ils ne fournissent ni fréquentation réelle ni historique assez long pour conclure sur des récurrences. La principale difficulté supplémentaire a été de ne pas confondre un indicateur calculable — comme les passages par habitant — avec une mesure directe de la qualité de desserte ou de la demande.
