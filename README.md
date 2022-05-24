# UE4-TPSgame
ps：游戏实机演示画面已经上传至bilibili  
本文的内容请在vpn开启状态下浏览（图片不清晰可以ctrl+滚轮放大观看哦）

## 主要模块


|------|
|[1.实机演示视频](#Update)  |
|[2.面试中遇到的向量计算以及视野问题](#面试中遇到的向量计算以及视野问题) |
|[3.射击逻辑射线检测](#射击逻辑-射线检测)   |


[1.实机演示视频](#Update)  
[2.面试中遇到的向量计算以及视野问题](#面试中遇到的向量计算以及视野问题)  
[3.射击逻辑射线检测](#射击逻辑-射线检测)  


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
### 射击逻辑 射线检测


