# PUBG-hack-detection

This project identifies cheaters in the online game PlayerUnknown’s BattleGrounds by using the k-means clustering algorithm. Players will be classified as normal players or cheaters based on their data. Then developers can use the trained model to predict potential cheaters through their game data. 

![PUBG](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/images/PUBG.jpg)

## Motivation

Our motivation to do this project is because we wanto to solve a real-world problem. A lot of my friends and I have been annoyed by cheaters in PUBG for a very long time. Once there is a cheater in the game, all other 99 players will have a bad game experience. Cheaters are so rampant but the company cannot do anything except to ban suspicious players. Because of the mechanism of FPS game, it is almost impossible to disable hack. To decrease the delay in a FPS game, developers must allow the game to compute and save all data in the client side. Since there is no way to prevent that scripts read data from the cache. Hack in FPS game is unavoidable. Therefore, we decided to train a model to help the game company identify hack in the PUBG.

## Procedure

### Collecting data

We used two parts of data as the train set and test set. 

* 500 players' data from [pubg.me/](pubg.me/). It contains top 100 users in ranking of five servers: as, na, eu, oc, and sea. After observation, we easily find a large number of abnormal data, which could be easily recognized as cheater’s data
	
* 85000 data from [https://www.kaggle.com/lazyjustin/pubgplayerstats](kaggle.com). Most data in this data set come from normal players. 
	
In order to balance the proportion of cheaters and achieve a better training effectiveness, we took all ranked data and 2000 data from normal data set and mix together as the training set. In this case, the training data set could contain both normal data and data from cheaters. 

The reason we mix data from the normal data set is to increase the density of normal players, and make the clustering tight. After test, we think 2000 rows of data has the best performance. 

### Feature selection

We tried to use feature selection algorithm such as PCA, but they showed extremely bad performance. Then we decided to select the feature manually.

After interview with some experienced players, we learned that there are two majority hacks: aimbot and waller. Aimbot helps players to lock the front sight of the gun on the enemy then they can easily kill. The waller allows players to see all enemies on the map. Therefore cheaters can gain advantage from asymmetric information. 

From the interview, we also learned that there are four features are most important to represent cheaters: win ratio, kill death ratio, headshot ratio, and top 10 ratio. After experiment, we are satisfied with their performance. 

### Module training

After trying couples of algorithm, we selected K-means to be the model, and we found that divide to 6 clusters have the best performance. 



