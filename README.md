# PUBG-hack-detection

This project identifies cheaters in the online game PlayerUnknown’s BattleGrounds by using the k-means clustering algorithm. Players will be classified as normal players or cheaters based on their data. Then developers can use the trained model to predict potential cheaters through their game data. 

![PUBG](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/images/PUBG.jpg)

## Motivation

Our motivation to do this project is because we want to solve a real-world problem. A lot of my friends and I have been annoyed by cheaters in PUBG for a very long time. Once there is a cheater in the game, all other 99 players will have a bad game experience. Cheaters are so rampant but the company cannot do anything except to ban suspicious players. Because of the mechanism of FPS game, it is almost impossible to disable hack. To decrease the delay in a FPS game, developers must allow the game to compute some data in the client side. Since there is no way to prevent that scripts read data from the cache. Hack in FPS game is unavoidable. Therefore, we decided to train a model to help the game company identify hack in the game.

## Procedure

### Collecting data

We used two parts of data as the train set and test set. 

* 500 players' data from [pubg.me/](pubg.me/). It contains top 100 players in ranking of five servers: as, na, eu, oc, and sea. After observation, we easily find a large number of abnormal data, which could be recognized as cheater’s data
	
* About 85000 data from [https://www.kaggle.com/lazyjustin/pubgplayerstats](kaggle.com). Most data in this set come from normal players. 
	
In order to balance the proportion of cheaters and achieve a better training effectiveness, we took all ranked data and 2000 data from Kaggle and mix together as the training set. In this case, the training data set could contain both normal data and data from cheaters. 

The reason we mix data from the normal data set is to increase the density of normal players, and make the clustering tight. After test, we think 2000 rows of data has the best performance. 

### Feature selection

We tried to use feature selection algorithm such as PCA, but they showed extremely bad performance. Then we decided to select the feature manually.

After interview with some experienced players, we learned that there are two majority hacks: aimbot and waller. Aimbot helps players to lock the front sight of the gun on the enemy then they can easily kill. The waller allows players to see all enemies on the map. Therefore cheaters can gain advantage from asymmetric information. 

From the interview, we also learned that there are four features are most important to represent cheaters: win ratio, kill death ratio, headshot ratio, and top 10 ratio. After experiment, we are satisfied with their performance. 

### Module training

After trying couples of algorithm, we selected K-means to be the model, and we found that dividing to 6 clusters have the best performance. 

![Training Image 1](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/images/training_1.png)

![Training Image 2](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/images/training_2.png)

We classified data to 6 clusters: normal player, experienced player, waller, aimbot, both, god.

* Normal player is the majority in the data set. It is low in all features.

* Experienced player is a speical cluster that we added after test. It has no difference with normal players except the top 10 ratio. This kind of players know how to survive to the end of the game. If we don't classify it as a new cluster, it will be classified as aimbot. It is also possible that this cluster contains cheaters, but they know how to hide themselves.

* Waller helps players to see all enemies on the map then players can avoid danger. Therefore it has high win ratio and top 10 ratio but normal KDR and headshot kill ratio.

* Aimbot helps players to automatically shoot on the target. Players with aimbot can easily kill a lot of people. Therefore it has high KDR and headshoot kill ratio. However, these players can be easily killed from their back, so there win ratio is not high. 

* Both means players who used both waller and aimbot. This kind of player can dominate the game. It is high in all features. 

* God. We have to classify it as a seperate cluster because its data are ridiculious. Their KDR is higher than 100, which means a player can kill all of other players in a game. They may used a powerful version of waller and aimbot. 

### Prediction

We used the rest of data from Kaggle.com as the test set and generated a reasonable result.

![Test Image 1](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/images/test_1.png)

![Test Image 2](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/images/test_2.png)

The result shows that there are 59252 normal players, 20398 experienced players, 3883 waller, 1369 aimbot, 896 both and 1 god in 85899 players' data. Since there is no way to verify the correctness of the prediction, we manually analyzed all kinds of hacks. Data that are classifed as hack are beliveable. However, we don't know if there are still hack in the normal or experienced players data set. 

The porprotion of hack is about the same what I thought (Since hack is very expensive). In the data set, most hack only played less than 10 games. I think they have been banned by the Bluehole.

## Code

The code of training is in the [PUBG_hack_detection.ipynb](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/PUBG_hack_detection.ipynb). To run it, install jupyter notebook and use

	jupyter notebook PUBG_hack_detection.ipynb
	
Before running, make sure you have installed numpy and sklearn. 

## Reference Paper

* Abbas, Osama. “Comparisions Between Data Clustering Algorithms.” The International Arab   Journal of Information Technology, vol. 5, July 2008, pp. 320–325., doi:10.18411/a-2017-023.

* “Which clustering method should you choos...” XLSTAT, 18 July 2016, help.xlstat.com/customer/en/portal/articles/2062432-which-clustering-method-should-you-choose-?b_id=9283.

* Bauckhage, Christian, et al. ClusteringGameBehaviorData. Clustering Game Behavior Data - IEEE Journals & Magazine, IEEE, 4 Dec. 2014, ieeexplore.ieee.org/abstract/document/6975073/. 

* Bouman, Charles. “CLUSTER: An Unsupervised Algorithm for Modeling Gaussian Mixtures.”  July 2005, engineering.purdue.edu/~bouman/software/cluster/manual.pdf.  

* Yang, Xing, et al. “Recognizing Driver Braking Intention with Vehicle Data Using Unsupervised Learning Methods.” 28 Mar. 2017, saemobilus-saeorg.ezproxy.lib.vt.edu/content/2017-01-0433/#authors.  

* Huang, Zhexue. “Extensions to the k-Means Algorithm for Clustering Large Data Sets with Categorical Values.” SpringerLink, Kluwer Academic Publishers, link.springer.com/article/10.1023%2FA%3A1009769707641?LI=true. 

* Amershi, Saleema. “Using Feature Selection and Unsupervised Clustering to Identify Affective Expressions in Educational Games.” 2016, saleemaamershi.com/papers/ITS2006_affective.pdf. 

