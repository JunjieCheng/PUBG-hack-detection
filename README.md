# PUBG-hack-detection

This project identifies cheaters in the online game PlayerUnknown’s BattleGrounds by using the k-means clustering algorithm. Players will be classified as normal players or cheaters based on their data. Then developers can use the trained model to predict potential cheaters through their game data. 

![PUBG](https://github.com/JunjieCheng/PUBG-hack-detection/blob/master/images/PUBG.jpg)

## Motivation

Our motivation to do this project is because we wanto to solve a real-world problem. A lot of my friends and I have been annoyed by cheaters in PUBG for a very long time. Once there is a cheater in the game, all other 99 players will have a bad game experience. Cheaters are so rampant but the company cannot do anything except to ban suspicious players. Because of the mechanism of FPS game, it is almost impossible to disable hack. To decrease the delay in a FPS game, developers must allow the game to compute and save all data in the client side. Since there is no way to prevent that scripts read data from the cache. Hack in FPS game is unavoidable. Therefore, we decided to train a model to help the game company identify hack in the PUBG.

## Procedure

### Collecting data

We crawled some players’ data of the game PlayerUnknown's Battlegrounds from the [pubg.me/](pubg.me/). The data consists of two parts:

* 500 data from top 100 users in ranking of each server: after our observation, we noticed a large number of abnormal data exist in this data set, which could be easily recognized as cheater’s data
	
* 85000 data from kaggle.com: almost all data in this data set is from normal players. We browsed those data and didn’t found any abnormal data.
	
In order to balance the proportion of cheaters and achieve a better training effectiveness, we took all ranked data and 2000 data from each data set randomly and mix together as the training set. In this case, the training data set could contain both normal data and data from cheaters. 
