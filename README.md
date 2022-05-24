# UE4-TPSgame
ps：游戏实机演示画面已经上传至bilibili  
本文的内容请在vpn开启状态下浏览（图片不清晰可以ctrl+滚轮放大观看哦）

## 主要模块
|主要模块|
|------|
|[1.实机演示视频](#Update)  |
|[2.test中遇到的向量计算以及视野问题](#test中遇到的向量计算以及视野问题) |
|[3.射击逻辑、射线碰撞检测](#射击逻辑射线碰撞检测)   |
|[4.手雷球型碰撞、伤害制作](#手雷球型碰撞伤害制作)   |
|[5.手雷的投掷以及爆炸推力](#手雷的投掷以及爆炸推力)   |
|[6.人物移动-状态机](#人物移动状态机)   |


[1.实机演示视频](#Update)  
[2.面试中遇到的向量计算以及视野问题](#面试中遇到的向量计算以及视野问题)  
[3.射击逻辑射线碰撞检测](#射击逻辑-射线碰撞检测)  


### test中遇到的向量计算以及视野问题
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
### 射击逻辑、射线碰撞检测  
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
为手雷定制一个蓝图：  
|手雷在诞生时执行以下操作|
|------|
|延迟1.5秒|
|spawn一个音效|
|在原地产生一个粒子特效|
|发射出球型射线|
|对每个射线碰撞到的actor发送一个指令（代表被击中的事件）|
|在原地产生一个球型击退效果（激活类中的force）|

![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E6%89%8B%E9%9B%B7%E5%88%B6%E4%BD%9C/%E6%89%8B%E9%9B%B7%E5%B1%95%E7%A4%BA.png)    
  
核心的球型射线算法，指的是从爆炸点中心向四周释放球型的多个射线（每个actor只会响应一次），需要提供中心坐标、半径，蓝图流程如下：  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E6%89%8B%E9%9B%B7%E5%88%B6%E4%BD%9C/%E6%89%8B%E9%9B%B7%E8%93%9D%E5%9B%BE1.png)    
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E6%89%8B%E9%9B%B7%E5%88%B6%E4%BD%9C/%E6%89%8B%E9%9B%B7%E8%93%9D%E5%9B%BE2.png)    
在AI类中收到被击中的指令之后，HP--20   

### 手雷的投掷以及爆炸推力   
手雷的产生源自于玩家类中的Q按键而产生，在收到键盘事件后，根据玩家的坐标以及向前朝向的向量计算出手雷的起始地点，再为其赋予一个向前的初速度。  
初速度与玩家的当前视野朝向的x、y分向量相同，z轴的值略微提高。  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E6%89%8B%E9%9B%B7%E7%9A%84%E6%8A%95%E6%8E%B7%E4%BB%A5%E5%8F%8A%E7%88%86%E7%82%B8%E6%8E%A8%E5%8A%9B/%E6%8A%95%E6%8E%B7%E8%93%9D%E5%9B%BE.png)    
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E6%89%8B%E9%9B%B7%E7%9A%84%E6%8A%95%E6%8E%B7%E4%BB%A5%E5%8F%8A%E7%88%86%E7%82%B8%E6%8E%A8%E5%8A%9B/%E6%8A%95%E6%8E%B7%E8%93%9D%E5%9B%BE.png)    
  
手雷在结束完对每一个检测到的AI发送事件后，在原地产生一个从中心向外的推力  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E6%89%8B%E9%9B%B7%E7%9A%84%E6%8A%95%E6%8E%B7%E4%BB%A5%E5%8F%8A%E7%88%86%E7%82%B8%E6%8E%A8%E5%8A%9B/%E7%88%86%E7%82%B8%E6%8E%A8%E5%8A%9B%E8%93%9D%E5%9B%BE.png)  
  
推力的参数设置如下：  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E6%89%8B%E9%9B%B7%E7%9A%84%E6%8A%95%E6%8E%B7%E4%BB%A5%E5%8F%8A%E7%88%86%E7%82%B8%E6%8E%A8%E5%8A%9B/%E6%8E%A8%E5%8A%9B%E5%8F%82%E6%95%B0.png)  

### 人物移动状态机    
状态机决定了人物在何种情况下进入什么样的移动姿态，从初始的站立状态到其他各个状态的转变，主要由按键以及移动速度来决定。  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E4%BA%BA%E7%89%A9%E7%A7%BB%E5%8A%A8%E7%8A%B6%E6%80%81%E6%9C%BA/%E7%8A%B6%E6%80%81%E6%9C%BA%E8%93%9D%E5%9B%BE.png)  
  
例如从站姿到蹲姿，需要用到ctrl键来触发，站姿到瞄准，需要用到右键来触发。  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E4%BA%BA%E7%89%A9%E7%A7%BB%E5%8A%A8%E7%8A%B6%E6%80%81%E6%9C%BA/%E7%8A%B6%E6%80%81%E8%B7%B3%E8%BD%ACnew.png)  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E4%BA%BA%E7%89%A9%E7%A7%BB%E5%8A%A8%E7%8A%B6%E6%80%81%E6%9C%BA/%E7%8A%B6%E6%80%81%E8%B7%B3%E8%BD%AC2.png)  
  
每个状态都有相应的骨骼动画与之绑定，决定了人物的动作，这些骨骼动画有些是纯动画，有些可以根据骨骼来混合动画。
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E4%BA%BA%E7%89%A9%E7%A7%BB%E5%8A%A8%E7%8A%B6%E6%80%81%E6%9C%BA/%E6%B7%B7%E5%90%88%E5%8A%A8%E7%94%BB.png)  
  
有些人物的移动设计没有放在状态机当中，而是通过直接设置人物移动的参数（如maxwalkspeed）来实现  
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/%E4%BA%BA%E7%89%A9%E7%A7%BB%E5%8A%A8%E7%8A%B6%E6%80%81%E6%9C%BA/%E8%B7%91%E6%AD%A5.png)  










