# UE4-TPSgame
ps：游戏实机演示画面已经上传至bilibili  
本文的内容请在vpn开启状态下浏览（图片不清晰可以ctrl+滚轮放大观看哦）

## 主要模块
|主要模块|
|------|
|[1.实机演示视频](#Update)  |
|[2.面试中遇到的向量计算以及视野问题](#面试中遇到的向量计算以及视野问题) |
|[3.射击逻辑-射线碰撞检测](#射击逻辑-射线碰撞检测)   |
|[4.手雷球型碰撞、伤害制作](#手雷球型碰撞、伤害制作)   |



[1.实机演示视频](#Update)  
[2.面试中遇到的向量计算以及视野问题](#面试中遇到的向量计算以及视野问题)  
[3.射击逻辑射线碰撞检测](#射击逻辑-射线碰撞检测)  


### 面试中遇到的向量计算以及视野问题
1）计算人物的视野方向与AI方向所成的夹角：  
获取人物的camera朝向、起始坐标、AI坐标得到两组向量  
利用公式：cos(θ)=α·β/(|α|·|β|)计算出角度θ  
顺逆时针可以通过两个vec叉乘，之后判断所得结果向量的Z轴的值是否大于0，若是则说明从俯视看是逆时针方向。 
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/Image-%E9%9D%A2%E8%AF%95%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E5%90%91%E9%87%8F%E8%AE%A1%E7%AE%97%E4%BB%A5%E5%8F%8A%E8%A7%86%E9%87%8E%E9%97%AE%E9%A2%98/%E8%AE%A1%E7%AE%97%E5%90%91%E9%87%8F%E5%B1%95%E7%A4%BA.png) 
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/Image-%E9%9D%A2%E8%AF%95%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E5%90%91%E9%87%8F%E8%AE%A1%E7%AE%97%E4%BB%A5%E5%8F%8A%E8%A7%86%E9%87%8E%E9%97%AE%E9%A2%98/%E8%AE%A1%E7%AE%97%E5%90%91%E9%87%8F.png)  
  
2）判断是否在敌人的视野之内  
根据上述的算法，可以从AI的角度出发看玩家，计算出AI目光与玩家所成夹角  
我们不妨设两个约束条件 距离小于1000码，角度绝对值小于45度，同时满足则视为看到玩家。  
值得一提的是，当AI死亡后，它的类将消失，那么该算法不再生效。  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/Image-%E9%9D%A2%E8%AF%95%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E5%90%91%E9%87%8F%E8%AE%A1%E7%AE%97%E4%BB%A5%E5%8F%8A%E8%A7%86%E9%87%8E%E9%97%AE%E9%A2%98/%E5%88%A4%E6%96%AD%E6%98%AF%E5%90%A6%E5%9C%A8%E6%95%8C%E4%BA%BA%E8%A7%86%E9%87%8E%E4%B9%8B%E5%86%85%E5%B1%95%E7%A4%BA.png)  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/Image-%E9%9D%A2%E8%AF%95%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E5%90%91%E9%87%8F%E8%AE%A1%E7%AE%97%E4%BB%A5%E5%8F%8A%E8%A7%86%E9%87%8E%E9%97%AE%E9%A2%98/%E5%88%A4%E6%96%AD%E6%98%AF%E5%90%A6%E5%9C%A8%E6%95%8C%E4%BA%BA%E7%9A%84%E8%A7%86%E9%87%8E%E4%B9%8B%E5%86%85.png)  
### 射击逻辑 射线碰撞检测  
从玩家的摄像机视角出发，需要调用camera的worldlocation、forwardvector  
绘制出一条射线并与AI的mesh进行射线碰撞检测  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E5%B0%84%E7%BA%BF%E6%A3%80%E6%B5%8B/%E5%B0%84%E7%BA%BF%E6%A3%80%E6%B5%8B%E5%B1%95%E7%A4%BA.png)  
  
对检测到的所有actor强制类型转换为AI类，调用AI类中的HP（float型）使其自减20.0  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E5%B0%84%E7%BA%BF%E6%A3%80%E6%B5%8B/%E5%B0%84%E7%BA%BF%E6%A3%80%E6%B5%8B.png)  
  
实现射线检测需要在UE4中创建一个通道（命名为attack）  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E5%B0%84%E7%BA%BF%E6%A3%80%E6%B5%8B/%E5%88%9B%E5%BB%BA%E9%80%9A%E9%81%93.png)  
  
还需要使AI类支持该射线检测（trace）才可以产生碰撞  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E5%B0%84%E7%BA%BF%E6%A3%80%E6%B5%8B/%E5%93%8D%E5%BA%94%E6%A3%80%E6%B5%8B.png)  

### 手雷球型碰撞、伤害制作

