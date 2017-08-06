---
layout: post
category: coding
title:  "小兔子搬萝卜问题解决方案"
tags: [算法]
---

> 有一只兔子喜欢吃萝卜，于是他从离家到50m的萝卜地，搬运100跟萝卜，但他很贪吃，每隔1m吃一只(返程也吃),
但每次只能云50只，问，他运到家最多还剩几只萝卜？

<!-- more -->
推广开来问题如下图所示:小白兔从距离家(HOME)distance= ***L*** 米处的萝卜地(FARM)搬运 ***M*** 个萝卜回家,他每次最多搬运50个萝卜.小白兔首先将M个萝卜分多次
搬运到距离家最近的X处并保证在X处的萝卜数量 ***M_X<=50*** 然后在X处将萝卜一次性搬回家.所以X距离FARM越远小白兔带回家的萝卜越多,但需要满足 ***X<=24***
不然小白兔回去的路上就没有萝卜吃了,或者一来一回在路上把萝卜全消耗光了.


~~~

FARM_________X_________HOME
   |------distance-----|

~~~

则小兔子从FARM走到X处需要走 (M/50)次，由于除最后一次不需要折返之外其他几次都需要折返，所以小兔子往返总共需要走次数 ***T=(M/50)*2-1***   
此时X点处萝卜总数为 ***M_X=M-T*X*** 个

若M_X<=50小兔子把M_X个萝卜由X点运到HOME  
若M_X>50小兔子还需要重复上面的过程,此时相当于FARM在X处

代码实现如下:

~~~ java

public class RabitCarryCarrot {
	/**
	 * 小白兔在距离家distance米处收获了carrotAmount个萝卜并准备运回家，但是小白兔很贪吃，每走一米就要吃一个萝卜（返程也要吃），问他能拿回家多少个萝卜？
	 * */

	private static int rabitCarryCarrot(int carrotAmount,int distance){
		int times = (int) (Math.ceil(2*(double)carrotAmount/50)-1);
		int distance_x=(int) Math.ceil(((double)carrotAmount-50)/times);
    	if(distance_x>=25){
    		distance_x=24;
    	}
    	System.out.println("最大折返距离："+distance_x);
    	int amountAtDistance_x =carrotAmount-times*distance_x;
    	System.out.println("最大折返距离处的萝卜输量："+amountAtDistance_x);
    	if(amountAtDistance_x<=50){
    		int result = carrotAmount-distance-(times-1)*distance_x;
    		if(result>0){
    			System.out.println("最后拿到家的萝卜数量："+result);
    			return result;
    		}else{
    			System.out.println("小白兔被兔妈妈打死了,因为它把萝卜全TM吃了");
    			return 0;
    		}
    	}else{
    		return rabitCarryCarrot(amountAtDistance_x,distance-distance_x);
    	}
	}


    public static void main(String[] args) {
    	Scanner sc=new Scanner(System.in);
    	System.out.println("萝卜数目");
    	int M=sc.nextInt();
    	System.out.println("距离");
    	int L=sc.nextInt();
    	System.out.println(rabitCarryCarrot(M,L));
    }
}

~~~

一些运行结果:  

~~~

萝卜数目
100
距离
50
最大折返距离：17
最大折返距离处的萝卜输量：49
最后拿到家的萝卜数量：16
16

萝卜数目
200
距离
50
最大折返距离：22
最大折返距离处的萝卜输量：46
最后拿到家的萝卜数量：18
18

萝卜数目
10000
距离
50
最大折返距离：24
最大折返距离处的萝卜输量：424
最大折返距离：24
最大折返距离处的萝卜输量：40
最后拿到家的萝卜数量：38
38

萝卜数目
100
距离
100
最大折返距离：17
最大折返距离处的萝卜输量：49
小白兔被兔妈妈打死了,因为它把萝卜全TM吃了
0

~~~
