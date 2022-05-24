# UE4-TPSgame
ps：游戏实机演示画面已经上传至bilibili  
本文的内容请在vpn开启状态下浏览（图片不清晰可以ctrl+滚轮放大观看哦）

## 主要模块

[1.实机演示视频](#Update)

[2.面试中遇到的向量计算以及视野问题](#面试中遇到的向量计算以及视野问题)

### 面试中遇到的向量计算以及视野问题
1）计算人物的视野方向与AI方向所成的夹角：  
获取人物的camera朝向、起始坐标、AI坐标得到两组向量 
利用公式：cos(θ)=α·β/(|α|·|β|)计算出角度θ 
顺逆时针可以通过两个vec叉乘，之后判断所得结果向量的Z轴的值是否大于0，若是则说明从俯视看是逆时针方向。 
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/Image-%E9%9D%A2%E8%AF%95%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E5%90%91%E9%87%8F%E8%AE%A1%E7%AE%97%E4%BB%A5%E5%8F%8A%E8%A7%86%E9%87%8E%E9%97%AE%E9%A2%98/%E8%AE%A1%E7%AE%97%E5%90%91%E9%87%8F%E5%B1%95%E7%A4%BA.png) 
![](https://github.com/Neko-yc/UE4-TPSgame/blob/main/Image-%E9%9D%A2%E8%AF%95%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E5%90%91%E9%87%8F%E8%AE%A1%E7%AE%97%E4%BB%A5%E5%8F%8A%E8%A7%86%E9%87%8E%E9%97%AE%E9%A2%98/%E8%AE%A1%E7%AE%97%E5%90%91%E9%87%8F.png) 




