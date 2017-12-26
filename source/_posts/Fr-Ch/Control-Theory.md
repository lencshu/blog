---
title: Control Theory
lang: Fr-Ch
date: 2017-12-26 01:36:40
tags:
mathjax: true
---

{% note info %}

!!! hint "总述"
		En automatique, un asservissement est un système dont l'objet principal est d'atteindre le plus rapidement possible sa valeur de consigne et de la maintenir, quelles que soient les perturbations externes.
		**`自动控制系统中，伺服系统的目的是，无论有何种外部干扰，都要尽可能快的达到并保持需求信号`**
		Le principe général est de comparer la consigne et l'état du système de manière à le corriger 		efficacement. On parle également de système commandé par rétroaction négative ou en boucle fermée.
		**`通常的原则是比较输入信号和系统状态来进行高效的纠正。通常又被称为负反馈或者闭环系统。`**

{% endnote %}

# 总结

# Part A 连续信号处理

## I) Placement des pôles 1948

**Conception des systèmes asservis linéaires par placement des pôles **
**通过放置极点来设计线性控制系统 1948**

### 1.1 目的
Objectifs de la méthode
当FTBO的Gain从0到正无穷时，求解特征方程的根(pôles de la FTBF)
### 1.2 分别回忆针对FTBO和FTBF零点与极点的关系
Rappel de la relation entre les pôles et les zéros de la FTBF 
![](images\cap_20171023_201655.png)

`F`onction `T`ransfert de `B`oucle `F`ermé :
![](images\cap_20171023_201953.png)

zero: 分子为零，即 `P1=0 or Q2=0`
pôle: 分母为零，即 求解特征方程的根 `Q1Q2+k1k2P1P2=0`
![](images\cap_20171023_213222.png)

alors，
![](images\cap_20171023_212408.png)
suivant,
![](images\cap_20171023_213222.png)

特征方程就可以写成
![](images\cap_20171023_213403.png)

!!! Question "Q1"
		![](images\cap_20171023_212640.png)
		Pourquoi `Zi` et `Pj`???

!!! Caution "结论"
		特征方程的根(FTBF的pôle)取决于取决于K的值，该值通过以下方程与FTBO的K相关联。而FTBF的零点不受K的影响
		![](images\cap_20171023_215832.png)
		图示
		![](images\cap_20171023_220909.png)

### 1.3 Lieu d’Evans 

由于伺服系统的性能直接取决于`开环系统`的K, 通常通过调试该数值以获取系统的其他指标参数。
当K从0变化到正无穷时，了解FTBF的pôle在复数平面的演变就非常重要。
FTBF的pôle是在空间一系列点，对应K值从小到大的变化。

这些位置就被称为`lieu d’Evans`，或`特征方程根的分布位置`，或**`FTBF的极值分布`**

#### 1.3.1 Conditions définissant le lieu d’Evans 

已知FTBO系统的极值Pj和零点Zi，当M点的坐标`p=a+jb`是特征方程`1+T[p]=0`的根。
可以写成：
![](images\cap_20171023_225534.png)
空间向量满足如下关系：
![](images\cap_20171023_225641.png)
该条件可翻译如下：
![](images\cap_20171023_225822.png)

!!! hint "注意"

		如果不存在零点，则分子为1

该条件可翻译成图像为：
![](images\cap_20171023_231344.png)

!!! Question "Q2"
		Z1 ？？？

RLtool de Matlab

#### 1.3.2  exemples a)
![](images\cap_20171023_232003.png)


!!! Note "解答"
		该FTBO：
		1. 无零点
		2. 有两个极值 `p1=0 p2=-4`
		3. 由特征方程`1+T[p]=0`可得多项式
		![](images\cap_20171023_232517.png)
		该多项式的零点是FTBF的pôle.
		把k的值从0到16每隔4取一个列表如下
		![](images\cap_20171023_235404.png)
			3.1 当k=0时，FTBO和BTBF的pôle相同
			3.2 当k=4时，实数值和虚数值的交界处，在该点处`ξ的值等于1`
			![](images\cap_20171024_000518.png)
			验证M的条件：
			![](images\cap_20171024_001455.png)

	!!! hint "复平面的性质"
			**不同的质量标准决定了阻尼amortissement：`ξ≥0.5`**
			**速度标准定义了这种关系：`ξω≥3/tr5%`**
			图示如下，
			![](images\cap_20171024_001312.png)

#### 1.3.2  exemples b)
Un système à contrôler a comme fonction transfert en boucle ouverte (avec retour 
unitaire): 
![](images\cap_20171030_192008.png)

- Matlab pôles dominants
![](images\cap_20171114_163857.png)

~~~c
clc
clear

num=1; %numérateur分子
den=conv([1 0],conv([1 2],[1 5]));  %denominateur
T=tf(num,den)
kesi=0.5; %Coef amortissement
omega0mini=1.43;  %从rltool图上可知natural frequence (pulsation propre)
p1=-kesi*omega0mini+1i*omega0mini*sqrt(1-kesi^2);
p2=-kesi*omega0mini-1i*omega0mini*sqrt(1-kesi^2);
%rltool
~~~
![](images\cap_20171111_135924.png)

!!! Danger ""
	点P3应在M点的左侧
![](images\cap_20171111_135255.png)

!!! hint "求pôles dominants公式:"
		![](images\cap_20171111_134515.png)

验证特征
![](images\cap_20171111_140754.png)

总结
![](images\cap_20171111_141153.png)


!!! attention "结论"
	En règle générale, il est admis que lorsque le module du pôle réel p3 reste supérieur ou égal à 10 fois le module de la partie réelle des racines complexes conjuguées, le troisième ordre est assimilable à un système du deuxième ordre. 
	当实数极值≥10倍复数极值的实数部分,那么三阶系统与二阶系统相似。




## II) Commande par retour d’état
**Conception des systèmes asservis linéaires par commande par retour d’état**
**通过状态反馈的线性伺服控制系统**

- `fonction de transfert` définie lors du cours d’`asservissements linéaires continus`.
- Le problème est traité dans le `domaine fréquentiel` et les `conditions initiales` sont `nulles`.
- La représentation d'état offre une `description plus complète` puisqu'elle permet de connaitre `l'évolution des variables internes du processus`
- `Il ne lui est pas demandé` de contrôler l'intensité du courant électrique(l’autre variable d'état i(t)) pourtant(cependant) responsable d'échauffements susceptibles de réduire notablement la durée de vie de son moteur. (不要求控制电流强度,但是对显著降低电机寿命的可接受发热负有责任)

### 2.1 转换成状态空间形式
Mise sous forme d’état (voir annexe ou cours MC53) 
![](images\cap_20171111_153234.png)

!!! hint "D"
	D est la matrice de transmission directe ou matrice de couplage entrées-sorties.Dans de nombreux cas elle est identiquement nulle puisqu'elle représente le cas particuliers des liaisons qui sont la limite du principe de causalité. 通常D为0，因为该矩阵表明特例情况下因果关系的限制关系

!!! Danger ""
	开环传递函数的极点就是系统矩阵A的特征值

#### 2.1.1  选择状态变量 
Sélection des variables d’état 

- Il faut d’abord inventorier l’ensemble des grandeurs qui apparaissent `sous forme dérivée` dans les équations. Ces grandeurs sont notées γi dont l’ordre maximum est noté ρi. Cette quantité définit le nombre de variables d’état qu’il faut introduire pour représenter une grandeur γi.
- L’ordre du système (linéaire ou non) est égal à la somme des ρi. ρi的数量就是系统阶数
首先要清查所有以微分形式出现的变量γi。同时每个变量其各阶量命名为ρi，该量定义了需要引入来表示某一γi量的状态变量的数量。
- Les entrées sont les variables indépendantes, alors que les sorties sont les grandeurs mesurées.Les grandeurs qui ne sont ni des variables d’état, ni des entrées, ni des sorties, doivent être éliminées par substitution. 输入量是互相独立的变量，输出量是测量量。其余无关量需要用替代法消除。 

##### 例题 3阶系统
![](images\cap_20171111_161009.png)

3阶系统整理可得
![](images\cap_20171111_161316.png)

#### 2.1.2  数学分析
Solution mathématique 

简化为一阶微分
![](images\cap_20171111_162704.png)
![](images\cap_20171111_163619.png)

#### 2.1.3  由转换方程到状态表示
Obtention d’une représentation d’état à partir d’une FT 

Soit un système considéré représenté par la FT(représentation dite externe) suivante : 
![](images\cap_20171111_163909.png)
![](images\cap_20171111_164257.png)
![](images\cap_20171111_164534.png)
于是
![](images\cap_20171111_164951.png)
![](images\cap_20171111_165528.png)

!!! hint "remarque"
	lorsque m = n, alors on procède à une division polynomiale pour obtenir une nouvelle FT, Go[p]  ,dont le d°N sera strictement inférieur à d°D. Ensuite on procède comme précédemment en utilisant la nouvelle FT Go[p] :
	![](images\cap_20171111_170022.png)
#### 2.1.4  状态表示到转换方程 
De la représentation d’état à la FT 
![](images\cap_20171111_170151.png)
### 2.2  系统的可控性和可观性 
Notion de Contrôlabilité et d’observabilité 

- La commande des systèmes asservis linéaires par commande par retour d’état va consister à 
placer les pôles du système en BF à l’endroit désiré sur le plan complexe afin d’atteindre les 
objectifs visés de stabilité, de rapidité, de précision...
通过状态方程的线性系统需求控制由在闭环系统的复数平面中放置满足要求的极点来达到稳定快速精确的目的。
- 为此，要求改系统可观可控。引入两个问题：
	-  Peut-on, en agissant sur les grandeurs d’entrée du système, faire passer l’état du système X(t) d’un état arbitraire X(to) à un autre état arbitraire X(t1)？vous vous posez donc à travers cette question basique le problème de la contrôlabilité. 
	`可控性：通过改变系统的输入变量，将系统状态从任意一个状态X(t0)变化到另一个任意状态X(t1)`
	-  Peut-on, en observant les grandeurs de sortie Y(t) du système sur un intervalle de temps suffisamment long [t0, t1], déduire l’état initial X(to) du système? avec cette  autre question tout aussi basique vous vous posez la problématique de l’observabilité.
	`客观性：通过观察在足够久的时间间隔内的系统输出，推断系统的输入状态`

#### 2.2.1 系统的可控性(可调性)
Contrôlabilité(ou Commandabilité)

- Kalman 于1960年引入的概念，以确定是否可通过`改变闭环系统的极值pôle`来达到设计目标
- 判定条件:Si à l’instant to le système se trouve à l’état initial X(to) = Xo, nous disons que cet état est commandable(ou contrôlable) si nous pouvons trouver un temps t1 fini supérieur à to et une commande U(t) dans l’intervalle de temps [to, t1] qui transfère l’état du système de Xo à X(t1)=X1. Nous disons aussi que le système est complètement commandable s’il est commandable quel que soit l’état initial Xo et l’instant initial to. 
	- t0时刻系统初始状态 X(t0)=X0, 如果存在t1(t1>t0)时刻使得某个控制量U(t)在[t0,t1]区间内可以把系统从状态Xo转变为X1,那么该系统为可控的。
	- 或者说如果该系统无论初始状态Xo或初始时刻是何值都是可控的，那么为完全可控。
- Pour les systèmes linéaires et invariants dans le temps, une condition nécessaire et suffisante de contrôlabilité a été donnée par Kalman et dépend uniquement de la paire (A, B). 对于线性时不变系统来说，kalman定义:可控性的充分必要条件只取决于(A,B). Cette condition se résume à ce que la matrice de commandabilité suivante :  
	
!!! attention "判定可控性的充分必要条件"
	$C_{o} = [𝑩, 𝑨𝑩 , …, 𝑨^{𝒏−𝟏}𝑩]$ soit de **rang(秩) n**
	Le paramètre n étant le nombre de variables d'état dans le vecteur d’état X
	**`秩就是矩阵中线性独立横行的数目`**

##### 举例说明
![](images\cap_20171111_213929.png)
![](images\cap_20171111_214241.png)

!!! Caution "可控性的判定条件"
	- **rank($C_{o}$)==rank(X)**
    - **det($C_{o}$) ≠ 0**	

#### 2.2.2 系统的可观性
Observabilité 

- Le concept d’observabilité est en quelque sorte le dual de celui de la commandabilité. En effet, l’observabilité consiste à déduire l’état initial X(t0) du système à partir des observations des grandeurs de sortie du système sur un intervalle de temps suffisamment long [t0, t1]. 
可观性的对偶概念是可控性。事实上，可观性需要从系统的输出量推断其在足够长的时间间隔上的初始状态。

!!! attention "判定可观性的充分必要条件"
	$O_{b} = [C; CA; CA^{2}; …; C^{𝒏−𝟏}A]$ soit de **rang(秩) n**
	Le paramètre n étant le nombre de variables d'état dans le vecteur d’état X (la dimension du vecteur état)
	**`秩就是矩阵中线性独立横行的数目`**

##### 举例说明
![](images\cap_20171111_213929.png)
![](images\cap_20171111_221139.png)

!!! Caution "可观性的判定条件"
	- **rank($C_{o}$)==rank(X)**
    - **det($C_{o}$) ≠ 0**	

#### 可控性可观性总结及matlab程序

!!! Attention ""
	- 可控制性是指可以利用输入将系统由初始状态转换成任意的最终状态
	- 可观察性是指系统的输出轨迹预测其初始状态

- Matlab observable controlable

~~~c
%On explicite les différentes matrices 
A=[0 1;-2 -3];
B=[0;1];
C=[1 0];
D=[0];
n=size(A,1);
%On lit la dimension n de notre vecteur d etat

%On les exprime sous forme d etat 
sys=ss(A,B,C,D);

%On lance la verification de la controlabilite 
CO=ctrb(sys);
nCO=rank(CO);
%On peut creer l interface suivante 
if rank(CO)==n 
    display('systeme controlable') 
    else; 
    display('systeme non controlable') 
end
%On lance la verification de l observabilite 
Ob=obsv(sys);
nOb=rank(Ob);
 
%On peut encore creer l interface suivante 
if rank(Ob)==n 
    display('systeme observable') 
    else; 
    display('systeme non observable') 
end 
~~~


### 2.3  Commande par retour d’état
反馈状态控制

#### 2.3.1  Présentation 

- 通过系统的状态来建立控制信号U(t)
- contrôler des systèmes de type MIMO
- 反馈状态控制的前提是假设所有的状态变量都是可以获取的。这种方法的代价是，状态变量越多，需要的传感器数量以及测量次数就变多了，结果就导致测量精度下降。 一个可替代的方案就是只测量较少的状态变量，然后通过状态观测器去推测其余的变量。Cette hypothèse deviendrait évidemment très onéreuse du fait que plus le nombre de variables d’état est élevé, plus le nombre de capteurs nécessaires à leurs mesures augmente. La fiabilité du système diminuant en conséquence. Une alternative à cette contrainte de coût consiste à ne mesurer qu’un nombre réduit de variables d’état et à estimer les autres au moyen d’un observateur d’état.
![](images\cap_20171113_130813.png)
- 虚线中为线性时不变系统的状态
- 通过设计参考信号r(t),反馈状态控制需要选择矩阵K
![](images\cap_20171113_131403.png)
- `De manière à` placer tous les pôles du système en boucle fermée à des positions bien précises. 为了在闭环控制系统中准确的放置极点，需要根据CDC的要求调节系统的动态特征。
- 矩阵N是个gain矩阵，用于调节闭环系统的gain statique，也就是静态特征。
- 联立开环系统模型以及u(t)的表达式可得
![](images\cap_20171113_133924.png)
- 对于极值Pôle Pi, i=1,2,...,n, 反馈系统的设计等价于解决下列代数方程：
![](images\cap_20171113_134354.png)

**为了解决这个问题，我们将分两种情况考虑**

- SISO（单输入单输出）--> 标量
- MIMO（多输入多输出）--> 多变量


#### 2.3.2  Etude d’un système SISO 

La méthode algébrique de résolution développée par `Ackermann` s’appuie sur le `théorème de Cayley-Hamilton`.
![](images\cap_20171113_140002.png)

- l’équation caractéristique désirée est aussi vérifiée par la matrice A

La formule d’Ackermann qui permet la détermination de la matrice K dans le cas scalaire est donnée par
![](images\cap_20171113_140251.png)

!!! Caution "重要"
	La présence de la matrice inverse de la contrôlabilité s’explique par le fait que le système doit être complètement contrôlable (à vérifier à chaque fois !!!).Il en résulte alors que la contrôlabilité du système est une condition nécessaire et suffisante à l’existence de la matrice K.
	可控性矩阵的逆矩阵要求系统完全可控（每次都要验证！）。矩阵K存在的充分必要条件是系统可控。

同时，$Co^{-1}$的存在也导致求解Ackermann方程的困难。Pour `contourner` cette difficulté:

- Trouver la matrice $d^{T} = (d_{1}, … , d_{n})$ en utilisant l’équation suivante :
$$d^{T}C_{0}  = [0, … ,0,0,1]$$
- Etablir la matrice de retour d’état en utilisant la relation suivante : 
$$𝐾 = d^{T}∆(𝐴)$$

对于之前的例子
![](images\cap_20171113_141640.png)

- 求解矩阵K同时满足如下条件:
	- Assurer un taux d’amortissement de l’ordre de 0,707 
    - Un temps de réponse à 5% de l’ordre de 2 s

解：
##### 2.3.2.1 方法一 Ackermann sur Cayley-Hamilton
表达式 
![](images\cap_20171113_142531.png)

Les pôles de notre second ordre sont évidemment de la forme : 
![](images\cap_20171113_142633.png)

où $\omega_{0}$ est déterminé à partir du temps de réponse puisque :  

**tr5% = 3/($\xi$$\omega_{0}$)** 



alors $\omega_{0}$= 3/($\xi$*tr5%) = 3/(0.7*2) = 2,12 rd/s

ce qui nous conduit à la valeurs de pôles :
P1,2 = −1.5 ± 𝑗1.5

l’équation caractéristique est alors donnée par : 
![](images\cap_20171113_143955.png)

La matrice de commandabilité Co
$$+$$
$$d^{T} = (d_{1}, d_{2})$$

donc,
$$d^{T}C_{0}  = [0,1]$$

donne,
![](images\cap_20171113_144612.png)

donc,
![](images\cap_20171113_144805.png)

##### 2.3.2.2 方法二 la méthode directe
Avec les pôles désirés : P1,2 = −1.5 ± 𝑗1.5 on obtient comme précédemment le polynôme : 
$P^{2}$ + 3P + 4.5 = 0
![](images\cap_20171113_145328.png)

比较系数可得：
3 + 𝑘2 = 3
2 + 𝑘1 = 4.5
##### 2.3.2.3 方法三 logiciel

- Matlab K de SISO

~~~c
A = [0 1;-2 -3] 
B = [0;1] 
p = [-1.5+1.5i;-1.5-1.5i]% pôles désirés 
K = place(A,B,p) 
~~~
sous Simulink:
![](images\cap_20171113_145646.png)

!!! Danger "重要"
	Simulink
	Pour le block de K, faut au'on utilise `Matrix(K*u)`

!!! hint "注意"
	我们也可以用代数的方法计算矩阵N(决定系统的静态特征)
	1. 闭环系统方程
	![](images\cap_20171113_150148.png)
	![](images\cap_20171113_150437.png)
	2. 假设输入r(t)为常数$r_{ref}$，那么y(t)=$r_{ref}$当t趋近无穷大。所以就有：
	![](images\cap_20171113_151035.png)

#### 2.3.3  Etude d’un système MIMO

例题
确定MIMO的`contre réaction K`

- Matlab K de MIMO

~~~c
clear;
clc;
A = [1 0 1;0 2 0;0 0 5]; 
B = [1 0;-1 0;1 1] ;
C = [1 0 0;0 1 0] ; %lecture de X1 et X2
D = [0 0;0 0] ;

%A有三个正根(特征向量)1,2,5, 表明该系统不稳定
eig(A);

%但是这个系统完全可控，因为矩阵Co的rang为3
sys=ss(A,B,C,D);
Co=ctrb(sys);
nCo= rank(Co);

%接下来通过放置pôle来稳定系统。用place命令可以解得K
p=[-1 -1.5 -1];
K=place(A,B,p);

%接着验证矩阵K是否正确地放置了FTBF的极点
AR=A-B*K
Poles = eig(AR)
%{
解得：
Pôles =
   -1.5000
   -1.0000
   -1.0000
%}

%求解N的值
N=1/(C*inv(-A+B*K)*B)
~~~

!!! hint "Netoyer"
	`clear`: workspace
	`clc`:console
!!! Question ""
	通过该示例可看出pole的选择接近无穷。如果想获得合理的结果，C’est en utilisant un critère d’optimisation quadratique qu’on répondra à cette question 需要使用二次优化原则。

#### 2.3.4  Reconstructeur d’état ou observateur

- 线性连续系统的稳定默认所有的变量都是可以获取的，但这几乎不存在。就算状态变量都是可测量的，传感器的延迟等因素也不能保证可以建立准确的状态反馈。
- 一个可能的方案就是借助额外的特制动态系统，设计一个状态反馈稳定器来评估所有的组成的状态向量
- 能够评估另一个动态系统的动态系统被称为评估器或者重建器，在一个决定的系统内，重建器又被叫做观测器(observateur de Luenberger)。然而在一个随即系统内部，它又被称作过滤器或者预测器。(filtre ou prédicteur de Kalman)
- 观测器的基本要求就是其必须是动态稳定的。
- 原则就是评估系统从输入量U(t)到输出量Y(t)的状态$\hat{X}$
![](images\cap_20171113_161445.png)

1. 第一步，构建一个跟原系统具有一样动态特征的系统：$\hat{\dot{X}}$(t)=A$\hat{X}$(t)+BU(t)
2. 减去实数模型可得：$\hat{X}$(t)-$\hat{\dot{X}}$(t)=A[X(t)-$\hat{X}$(t)] , 该方程与U(t)无关。
3. 评估误差：$\dot{e}(t)=Ae(t)$

由此可知，动态误差取决于A。所以如果矩阵A不稳定，那么误差也将随着时间变大。Luenberger提出一个把评估误差考虑在内的方法：
![](images\cap_20171113_163514.png)
事实上，右边的部分取决于评估误差：
![](images\cap_20171113_163803.png)
重新整理观测器的方程可得，
![](images\cap_20171113_163904.png)

所以观测器就是由上述方程描述，具有U(t)和Y(t)并有初始状态评估$\hat{X}$(t)的动态系统。
如果满足以下性质，
![](images\cap_20171113_164601.png)
那么该系统就是状态系统的渐进评估器（观察器）
结构如下：
![](images\cap_20171113_164928.png)

由于系统结构相同因此可以用计算k的方法来计算L。事实上，如果需求的pole能保证误差趋近于零，那么L的计算方法与K近似为：
![](images\cap_20171113_170722.png)
**Avec** $$ L = [L1;L2;...;Ln]$$
rappel:
![](images\cap_20171113_134354.png)
**Avec** $$ K = [K1,k2,...,Kn]$$

事实上，纠正器K的性能取决于状态的评估，所以显然状态评估值可以迅速变化为实际值。结果就是，观测器的pôle dominant会在纠正器correcteur的左侧。通常，倍率10就足够大到可以保证估计值可以收敛到实际值。

!!! danger ""
	确保系统是可控的！


例题

- Matlab L de MIMO

~~~c
clc;
clear;
%二阶的SISO系统
A = [2 3;-1 4];
B = [0;1];
C = [1 0];
D = [0] ;
sys=ss(A,B,C,D);

%系统是否可控，矩阵Co的rang是否为2
Co=ctrb(sys);
nCo= rank(Co);

%une pulsation ω de 10 rd/s,coefficient amortissement de 0.8, donc les poles:
p=[-8+6i;-8-6i];
%un polynôme caractéristique : p²+16p+100
%{
计算公式
kesi=0.8; %Coef d amortissement
%tr=0.5; %temps de reponse
omega0mini=10;  %  3/(kesi*tr);  
p1=-kesi*omega0mini+1i*omega0mini*sqrt(1-kesi^2);
p2=-kesi*omega0mini-1i*omega0mini*sqrt(1-kesi^2);
%}

L =place(A.',C.',p).'
%{
	或者
	At=transpose(A);
	Ct=transpose(C);
	L=place(At,Ct,p).'
%}

~~~

手算思路：
![](images\cap_20171113_174046.png)



#### 2.3.5  Observateur 状态反馈控制 
Commande par retour d’état avec observateur

- 通过反馈状态系统的pôle放置不进可以稳定线性连续系统而且可以通过对pôle位置的选择来指定某些动态表现。
- 如果我们想对一个状态不可测的系统S=(A,B,C)使用状态反馈，那就需要一个可以提供X(t)的估计值$\hat{X}$(t)的观测器。
![](images\cap_20171113_175739.png)

Les équations d’état et de sortie pour le système global sont donc：
![](images\cap_20171113_180148.png)
On considère que l’état du système global est formé par les variables intervenant dans les
vecteurs $\hat{X}$(t) et $\hat{\dot{X}}$
![](images\cap_20171113_180409.png)

Ces deux équations peuvent être écrites sous la forme habituelle associée à un système linéaire continu, soit :
![](images\cap_20171113_181555.png)

!!! hint "remarque"
	如果系统完全可测，那么观测器可以写成
	![](images\cap_20171113_182308.png)
	F1和F2相同 --> 观测器在输入输出端不可见
	Ces deux fonction de transfert F1[p] et F2[p] sont identiques. Ce qui veut dire que l’observateur est « invisible » du point de vue entrée-sortie.



!!! unknow "Procédure de conception d’un retour d’état avec observateur : "
	!!! note "Première étape :"
		**第一步：可控性**
		Vérifier la commandabilité et l’observabilité du système
	!!! note "Deuxième étape : "
		**第二步：通过选取pôle(根据CDC)来决定矩阵K**
		Déterminer  la  matrice  de  contre  réaction  K  en  prenant  des  positions  de  pôles  qui  vous semblent les plus intéressantes par rapport au cahier des charges.(amortissement et temps de réponse). 
	!!! note "Troisième étape : "
		**第三步：放大10倍作为pôle来计算矩阵L**
		Déterminer la matrice L en choisissant des pôles dominants dans un rapport de 10 par rapport aux pôles dominant du système. 
	!!! note "Quatrième étape :"
		**第四步：构建系统**
		Construire votre système global comme sur la figure ci-après. 
		![](images\cap_20171113_183117.png)
	!!! note "Cinquième étape :"
		**第五步：分析并调整pôle**
		Analyser les résultats et réajuster les placements des pôles si besoin.



#### 2.3.6 Intégrale 状态反馈控制
Commande par retour d’état avec action intégrale

- 反馈状态变量调节的不足之处是只能具有比例和微分。因此就不能消除最终误差incapable si le besoin s’en fait sentir d’annuler l’erreur en régime permanent. 因此要加入一个积分部分。On utilisera un correcteur de type intégral que l’on positionnera comme indiqué sur la figure ci-après.
![](images\cap_20171113_183709.png)

- 由于同质性的原因，我们把误差表示为e(t)–y(t)，-$\dot{X}_{i}(𝑡)$。符号是为了获取同质表达式。
- 积分作用增加了状态模型的维度：
$\dot{X}(𝑡)$=𝐴𝑋(𝑡) + 𝐵𝑈(𝑡) avec X(0) = X0
𝑌(𝑡) = 𝐶 𝑋(𝑡)
$\dot{X}_{i}(𝑡)$ = 𝑌(𝑡) − 𝑒(𝑡)
𝑈(𝑡) = −𝐾𝑖𝑋𝑖(𝑡) − 𝐾𝑋(𝑡)   La loi de commande implantée
d'où 
![](images\cap_20171113_190756.png)
综上，重新写成:
![](images\cap_20171113_192722.png)

$$\dot{Z}_{(t)}=(\hat{A}-\hat{B}\tilde{K})Z(t)+\hat{D} e(t)$$

!!! hint "l’objectif de la commande est double"

	1. Assurer la stabilité du système augmenté ( et donc plus particulièrement le système original) en boucle fermée
	2. Assurer une erreur nulle en régime permanent :
	![](images\cap_20171113_195506.png)

- 增加后的系统的稳定和误差收敛到0的速度取决于det(k)，也就是选择的$\hat{A}-\hat{B}\tilde{K}$的极点。事实上，加了积分部分的状态反馈控制等价于对于增维的系统($\hat{A},\hat{B},\hat{C},0$)进行经典的状态反馈分析。

例题
$$G_{[p]}=\frac{1+p}{(p+2)(p+3)}$$

- Déterminer la forme d’état
- Vérifier la contrôlabilité et l’observabilité
- Déterminer K en prenant comme pôles désirés (-1 ;-8 ;-9) 
- Dessiner la réponse indicielle
![](images\cap_20171122_183014.png)
![](images\cap_20171122_183237.png)


## III) LQR 

**Conception des systèmes asservis linéaires par régulation linéaire quadratique**

- 该调节也被称为最优调节Contrôle Optimal
	- 达到最终需求的状态。
	- 优化时间。用最少的时间达到目标
	- 优化必要的控制能量
	- 在平衡位置调节系统
	- 跟随轨迹

- 在反馈状态调节的情况下，我们有无数种选择pole的可能。因此我们的选择对于不同的目标未必只最合适的。L Q调节目的就在此。

### 3.1 Critère d’optimisation quadratique 
#### 3.1.1 Présentation 
Soient deux systèmes monovariables du $1^{er}$ ordre:
两个单变量一阶系统
![](images\cap_20171113_204727.png)

通过比较两个图线$X^{2}_{i(t)}$来分别分析rapidités响应速度
![](images\cap_20171113_205148.png)

很容易得出，
![](images\cap_20171113_205241.png)

写成变量的形式，
![](images\cap_20171113_205458.png)

#### 3.1.2 Critère des performances Jx 
**$$性能标准Jx$$**
- 目前的问题是这两个参数X1,X2有相同的权重。绕过这个问题的方法是引入一个权重矩阵Q，这是个对称且为正的矩阵。
于是X(t)(系统的性能)根据二阶优化标准可以写成：
![](images\cap_20171113_212622.png)

例题
![](images\cap_20171113_213017.png)

#### 3.1.3 Critère d’économie énergétique Ju 
**$$控制信号的能量标准$$**
同理可得
$$J_{u}=\int_0^\infty U^{t}(t)RU(t)dt$$
- R 控制指令的能量权重矩阵，对称且为正

#### 3.1.4 Critère quadratique(de compromis) $J_{LQ}$. 
**$$二阶优化标准(妥协)$$**

- Le critère de compromis JLQ entre les performances du système et l’économie de l’énergie de commande:
$$J_{LQ}=J_{x}+J_{u}=\int_0^\infty X^{t}(t)QX(t)dt+\int_0^\infty U^{t}(t)RU(t)dt$$
为了更进一步突出优先级，性能优先或者控制能量优先，令R = ρR'，ρ是正标量，于是：

$$J_{LQ}=J_{x}+J_{u}=\int_0^\infty X^{t}(t)QX(t)dt+\int_0^\infty U^{t}(t)R'U(t)dt$$

所以：

- 如果ρ增大，那么Ju增大：节约控制能量的消耗
- 如果ρ减小，那么Jx的权重增大：提高性能

### 3.2 Solution du problème LQR
Solution du problème de la régulation linéaire quadratique. : équation de Riccati 
**$$LQR问题的解$$**

对于一线性不变系统$\dot{X}(t)=AX(t)+BU(t)$，问题的实质是：

- 计算K，使得状态反馈`u(t)=-Kx(t)`
- 同时，$J_{LQ}$最小

解决方案是Equation de RICCATI algébrique
$$PA+A^{t}P+Q-PBR^{-1}B^{t}P=0$$

矩阵P是该问题（稳定闭环系统）的唯一解（只取正值），K的值通过$K=R^{-1}B^{t}P$求得

例题
![](images\cap_20171113_221403.png)

- ①déterminer la matrice P de Riccati
- ②déterminer la matrice de contre réaction K
- ③de vérifier les valeurs de pôles de la FTBF.

解：
P的形式为：
![](images\cap_20171113_221738.png)
代入Riccati方程：
$$PA+A^{t}P+Q-PBR^{-1}B^{t}P=0$$
![](images\cap_20171114_100042.png)
donc,
![](images\cap_20171114_100302.png)
因为P是对称的，所以`P12=P21`，于是得到4个方程

(eq1) −2P12 + 2 − P122 = 0 
(eq2) P11 − 2P12−P22 − P12P22 = 0 
(eq3) −P22+P11 − 2P12 − P22P12 = 0 
(eq4) P12 − 2P22+P12 − 2P22 + 1 − $P22^{2}$ = 0 

事实上2和3是相同的−2P12 + 2 − $P12^{2}$ = 0。分别求解，只取正值on ne retient que la valeur positive：

	(eq1) P12 = 0.732 𝑒𝑡 − 2.732
	(eq4) P22 = 0.542 𝑒𝑡 − 4.452
	(eq2) P11 = 2 ∗ 0.732 + 0.542 + 0732 ∗ 0.542= 2.403
解得：①![](images\cap_20171114_101319.png)
②![](images\cap_20171114_101438.png)
③Les pôles étant les valeurs propres de la matrice (A-BK), 计算det(pI-A+BK)：
![](images\cap_20171114_101704.png)
解得：p1,2 = -1.27$\underline{+}$j0.341

- Matlab[K,P,E]
![](images\cap_20171114_185502.png)
~~~python
A=[0 1;-1 -2]
B=[0;1]
Q=[2 0;0 1]
R=[1] 
[K,P,E]=lqr(A,B,Q,R)
%{
	K:contre reaction
	P:la matrice de Riccati
	E:les valeurs de pôles de la FTBF
}
~~~

La fonction lqi：intégration avec le lqr

### 3.3 LQR应用
Procédure de mise au point d’une régulation linéaire quadratique. 

- 选择Q和R à choisir les facteurs des matrices Q et R.
- 用软件处理 faire mouliner le logiciel
- 分析结果 analyser les résultats puis
- 根据CDC反复调整参数 intervenir sur les facteurs et réitérer jusqu’à satisfaction des objectifs du CDC.

## IV)LQG
LQG介绍Introduction à la commande Linéaire Quadratique Gaussienne：

Le régulateur LQG est une combinaison entre un régulateur LQR et un estimateur de Kalman (LQG = LQR + filtre de Kalman). LQG的状态反馈环节包含一个`LQ`与一个`卡尔曼滤波器`


### 4.1 Principe du filtre de Kalman 

- 二阶线性控制用到了状态反馈。实际上，如果输出与状态不相符，那么就需要从给定的U和Y开始状态重建。 这个步骤在决定论déterministe的问题里对应的是observateur de Luenberger，而在随机stochastique的问题里就是filtre de Kalman卡尔曼滤波器。
- 卡尔曼滤波器是个对状态向量进行预测和评估的观测器，记为$\hat{X}$. 在随机幻境里，当变量的噪声已知，那就是最好的线性观测器。另外，如果白噪声是高斯噪声，那么对所有的观测器来说，观测误差变量是最小的。

例题
- 设随机变量V,W,X(t0)是高斯变量
- 求使二阶偏差的标准E[(x(t) − $\hat{X}$(t))^{t}(x(t)-$\hat{X}$(t))最小的$\hat{X}$(t), E是数学期望。

解：
先让评估误差均值最小(状态和评估的差值)
不展开介绍卡尔曼滤波器的理论，只介绍如何使在用连续系统建立好的结论。

方程和结果 Mise en équation et résultats
![](images\cap_20171027_105517.png)
- w 
    - 是n维的未知干扰。
    - 变换自均值为0的高斯白噪音模型，协方差W(非负的n*n维的矩阵)
    - 通常来说，W表示对于系统建模疑虑。

-V
	- 测量误差。
	- 变换自均值为0的高斯白噪音模型，协方差V(非负的p*n维的矩阵)

结构如下：
![](images\cap_20171114_143404.png)

方程为：
![](images\cap_20171114_144048.png)

开环系统的动态性能被闭环测量的评估所纠正。
- W代表状态噪声的协方差矩阵，换句话说，就是模型的可信度。
- V代表测量噪声的协方差矩阵，就是测量的精确度。

当测量有非常大的噪音时，V的值变大，可观测到在矩阵Kf里，增益gain变小。

### 4.2 类比LQ优化 
**$$Analogie avec le problème de la commande optimale LQ$$**

- 定义滤波器的方程被称为二阶优化问题的解。用p来表示的矩阵Kf是Riccati微分方程的解。
- 我们通过把A,B,L分别替换为At,Ct和Kf。同时，矩阵Q，R分别替换为协方差矩阵V,W。
- 另外，Riccati方程的积分针对的是实时情况。
Les équations définissant le filtre rappellent la solution d’un problème de commande optimale à critère quadratique. Ainsi la matrice Kf s’exprime en fonction de P, laquelle est la solution d’une équation différentielle de Riccati.
On obtient la solution du problème de l’estimation à partir de la solution du problème de commande en remplaçant les matrices A, B et L par respectivement At, Ct et Kf et les matrices de pondération Q et R du critère par les covariances des bruits V et W.
Par ailleurs, il convient de noter que l’intégration de l’équation de Riccati est effectuée ici en temps réel, à partir d’une condition initiale Po dans laquelle on introduit d’éventuelles connaissances à priori sur l’erreur d’estimation. Ceci permet d’en calculer la solution pas à pas en temps réel, ce qui facilite grandement  son incorporation(du filtre) dans une boucle de commande.

### 4.3 Mise en œuvre du filtre de Kalman 

- 假设我们已知状态协方差和噪声协方差的期望

On suppose connues l’espérance et la covariance de l’état initial ainsi que les covariances des bruits. En pratique, celles-ci ne sont pas connues de manière exacte.
On peut toutefois faire les remarques suivantes :
- lorsque le système est invariant, par analogie avec le problème de commande optimale, on peut chercher une valeur asymptotique constante de la matice P(t) en faisant tendre t vers l’infini ($\dot{P}(t)$ = 0). La différence avec le filtre optimal dépendant du temps se fera sentir en début de fonctionnement seulement, ce qui constitue un inconvénient mineur vis-à-vis de la simplification considérable apportée par l’utilisation d’une matrice P constante.
- L’observateur est sans biais 偏差
- Si V et W sont mal connues, le gain Kf peut être déterminé en considérant simplement ces matrices comme des matrices de pondération analogues aux matrices Q et R du problème de la commande optimale.
如果V和W未知，那么Kf可以通过矩阵Q，R来确定。

### 4.4 La commande LQG 

传统的LQG控制有两个步骤：对偶duales和独立indépendantes：

- 通过二阶优化的状态反馈，比如求Ko (un indice « o » pour bien le différencier / Kf)
- 从唯一可用的测量信号开始应用的卡尔文滤波器。比如求Kf。La synthèse d’un filtre de Kalman permettant d’implanter la commande à partir des seules mesures disponibles (modèle bruité ainsi que les mesures)

#### 4.4.1 Le principe de séparation

**$$分离原则$$**

当我们把LQ优化中的X(t)换成$\hat{X}(t)$会发生啥？其中$\hat{X}(t)$由卡尔曼滤波器求得。

滤波器的方程是：
![](images\cap_20171114_144048.png)

控制信号为：
![](images\cap_20171114_150609.png)

评估误差是：
$\varepsilon$= x − $\hat{X}(t)$

展开可得：
![](images\cap_20171114_151120.png)

如果A – K$_{f}$C全都位于复平面的左半边，那么这个系统就是异步asynchronement稳定，状态X(t)和$\hat{X}(t)$之间的向量误差呈指数函数地趋近于零。

为了实现输出反馈调节，可以用一个状态观测器和一个利用观测状态的状态反馈调节器
![](images\cap_20171114_152105.png)
于是，
![](images\cap_20171114_153412.png)

La matrice d’évolution est bloc-triangulaire. Ses valeurs propres sont les valeurs propres des blocs de la diagonale : [A−BKo],[A−KfC] . Les dynamiques du retour d’état d’une part, et de l’observateur d’autre part, sont séparées : on peut régler les valeurs propres de la commande par la matrice de retour d’état K, de façon indépendante des valeurs propres de l’observateur que l’on règle par le choix de la matrice Kf. C’est le principe de séparation.

!!! hint "重要结论"
	LQ和卡尔曼滤波器可以分开计算。
	Conclusion très importante :
	La commande par retour d’état LQ et le filtre de Kalman peuvent être déterminés séparément.

	!!! Note ""
		LQG的结构是由如下关系给定的
		![](images\cap_20171114_154228.png)
		或可图示如下
		![](images\cap_20171114_154334.png)

- Matlab LQG

	[Kf,P,E]=lqe(A,M,C,Qk,Rk)

#### 4.4.2 干燥高岭土 Séchage du Kaolin 
Exemple afin d’illustrer notre propos
![](images\cap_20171117_005459.png)

- 进料8吨/h; 要求出料湿度控制在4%到12%，最大方差为1%;
- 加热温度 tb(t) ℃
- 干燥温度 td(t) ℃
- 出料湿度 mf(t) %

控制参数
- 燃烧炉阀门开关 Va(t) rad
- 高岭土进料 fi(t) t/h
![](images\cap_20171117_010218.png)

求：LQG建模，控制并评估三个参数

解：
建模
![](images\cap_20171117_010643.png)

!!! hint ""
	负号意味着湿度与温度变化相反

合并最后两项得：
![](images\cap_20171117_010726.png)

状态空间表示
![](images\cap_20171117_010831.png)

C1和C2未知，是燃烧炉和干燥室的干扰，有随机数0.1完全决定。
Où c1 et c2 sont des coefficients inconnus liés aux perturbations sur le
bruleur et le séchoir et sont donnés de façon complètement aléatoire à une valeur de 0,1.

于是可以写成
![](images\cap_20171117_011327.png)
观测方程
![](images\cap_20171117_011407.png)

接下来要做的是

- 1.déterminer le gain optimal  Kopt du LQR
- 2.déterminer ensuite le gain du filtre de Kalman
- 3.Puis construire le contrôle LQG

1. R=1,Q=[q11 0 0; 0 q22 0; 0 0 q33]

目标是：
- Obtenir un taux d’humidité par exemple de 6%$\underline{+}$1% pour un débit de Kaolin de 8 tonnes/heure .La température du séchoir ne doit pas varier plus de ±3°C autour de 50°C.
- Les valeurs recherchées sont : Tb = 450 °C ; Td = 50°C et mf = -6%
- Remarque : le taux d’humidité est négatif du à la modélisation de départ.
- Les conditions initiales sont :
![](images\cap_20171117_011953.png)

~~~python
A=[-.0213 0 0; 0.0006 -0.005 0;0 -0.00038 -0.0023];
B=[8.9362;0;0];
M=[0.1 0 0;0 0.1 0;0 0 0.00132];
C=eye(3);
Q=[0 0 0;0 0.5 0;0 0 20];
R=1;
[K,S,E]=lqr(A,B,Q,R);
eig(A-B*K);
~~~

用simulink模拟
![](images\cap_20171117_012433.png)

可以明显的看出，需要集成一个积分纠正器
Le niveau haut de la saturation définissant l’ouverture maxi de la vanne est : π/2 
Le niveau bas : 0

!!! hint ""
	修改solver

接下来在LQ中加入建模和测量噪声来观察集成滤波器的必要性
![](images\cap_20171117_012759.png)

高斯噪音生成模块 Band-Limited White Noise
![](images\cap_20171117_013138.png)
![](images\cap_20171117_014152.png)

!!! hint ""
	Noise Power = Variance * Période d’échantillonnage


确定协方差矩阵V：
![](images\cap_20171117_014319.png)

确定协方差矩阵W
C’est une matrice diagonale dont les termes q11 et q22 représente des variations potentielles des températures du bruleur et du séchoir résultant du transfert de chaleur au niveau des parois du séchoir, du  vent et des variations externes de température. Il est, sans information suffisante, possible de penser que ces variations sont de l’ordre de 0.3°C.
Quant au débit de Kaolin, on peut penser que notre modèle peut être précis à 0.3 tonnes près.
![](images\cap_20171117_014444.png)

然后

~~~python
[Kf,P,E]=lqe(A,M,C, W,V)
~~~

可得：
![](images\cap_20171117_014702.png)
![](images\cap_20171117_021941.png)

- matlab kalman

~~~python
A=[-.0213 0 0; 0.0006 -0.005 0;0 -0.00038 -0.0023] ;
B=[8.9362;0;0] ;
M=[0.1 0 0;0 0.1 0;0 0 0.00132] ;
C=eye(3) ;
Q=[0 0 0;0 0.5 0;0 0 20] ;
R=1;
[K,S,E]=lqr(A,B,Q,R) ;
eig(A-B*K);

W=[0.1 0 0;0 0.1 0;0 0 0.1];
V=[0.1 0 0;0 0.01 0; 0 0 7.46];
[Kf,P,E]=lqe(A,M,C, W,V);
~~~


## V) Conception des systèmes asservis intelligents 
### 5.1 Logique floue 
### 5.2 Réseaux de neurones 
### 5.3 Algorithmes génétiques
## VI) Simulations numériques 
### 6.1 Software In the Loop :SIL
### 6.2 Hardware In the Loop :HIL

# 附录 A
## I 英法对照

!!! hint "英法对照"

	|法|英| 
	| ------------- |:---------------:| 
	|Temps de réponse à 5%(Tr5%)|setting time| 
	|Temps de montée (Tm10-90)|rise time|
	|Peak réponse de dépassement|peak ampititude, overshot,at time|
	|Erreur de position|final value|
	|Coef amortissement|damping raito|
	|pulsation propre $\omega$(rad/s)|natural frequence f*2$\pi$|

## II Matlab 公式

!!! hint "Matlab 公式"
		conv (A, B)
		roots( )
		p=poly(r)
		charpoly()

		传递函数 num,den
		对线性定常系统，s的系数均为常数，且den的常数项不等于0
		sys= tf (num, den)

		提取分子分母多项式系数的函数tfdata( )。格式：[num,den]=tfdata(sys, ‘v’)
		sys传递函数。v功能：返回分子分母多项式系数向量。

		siso零极点模型
		sys= zpk (z, p, k)
		Zero,pole,gain

		提取模型零极点增益向量的函数
		[z, p, k]=zpkdata(sys, ‘v’)

		建立状态空间模型的函数
		sys=ss(A,B,C,D)

		特征根 传递函数分母的根

		微分方程

		线性时不变系统（LTI）的模型：
		传递函数（Transfer Function）模型TF
		零极点增益（ZPK）模型ZPK
		状态空间（State Space）模型SS
		P33

		z，p向量 k标量

		模型连接：串联series，并联parallel

		求闭环传递函数的MATLAB函数cloop( )和feedback( )
		其中cloop( )函数只能用于H(s)=1（即单位反馈）
		[numc, denc]=cloop(num, den, sign)

		[num, den]=feedback(num1, den1, num2, den2, sign)
		num1, den1：G(s) 的分子、分母多项式
		num2, den2: H(s)的分子、分母多项式
		sign= -1 为负反馈（默认值），sign=1 为正反馈
		P44

		电感 微分器
		电容 积分器

		拉普拉斯变换

		阶跃响应
		step(tf)
		grid on


		利用simulink的提取线性模型函数 linmod( ), 得到状态空间模型，然后对状态空间模型进行各种仿真。

		[A,B,C,D]=linmod(‘samples_4_14’);
		[num,den]=ss2tf(A,B,C,D);
		printsys(num,den,’s’) %以传递函数形式显示出来



## TD习题
### TD01
#### Ex01
![](images\cap_20171106_205602.png)
![](images\cap_20171114_204910.png)

#### Ex02
##### a)
![](images\cap_20171114_205311.png)
![](images\cap_20171114_205323.png)

##### b)
![](images\cap_20171114_205754.png){:width="1200px"}
![](images\cap_20171114_205716.png){:width="1200px"}
![](images\cap_20171114_205915.png){:width="1200px"}

##### c)
![](images\cap_20171114_205947.png){:width="1200px"}
![](images\cap_20171114_210016.png){:width="1200px"}

##### d)
![](images\cap_20171114_210125.png){:width="1200px"}
![](images\cap_20171114_210155.png){:width="1200px"}
![](images\cap_20171114_210232.png){:width="1200px"}

##### e)
![](images\cap_20171114_210852.png){:width="1200px"}

### TD02
#### Ex01
![](images\cap_20171114_211804.png){:width="1200px"}
#### Ex02
![](images\cap_20171114_211918.png){:width="1200px"}
#### Ex03
![](images\cap_20171114_212009.png){:width="1200px"}
~~~c
num=4;
den=[1 3 6 2];
[A,B,C,D]=tf2ss(num,den)
~~~

#### Ex04
![](images\cap_20171114_212033.png)
~~~c
A=[0 1;-2 -3];
B=[0;1];
C=[5 1];
D=[0];
[num,den]=ss2tf(A,B,C,D);
syms p;
I=[1 0;0 1];
A0=(p*I-A);
A1=inv(A0)
~~~

#### Ex05
![](images\cap_20171114_212055.png)
~~~c
A=[2 0;-1 1];
B=[1;-1];
C=[1 1]; %faut C=[1 0;0 1];
D=[0];

sys=ss(A,B,C,D);

Co=ctrb(sys); % matrice de commandabilite
nCo=rank(Co) %determinant
Ob=obsv(sys)
nOb=rank(Ob)
~~~

### TD03 Chariot et Prehenseur
![](images\cap_20171114_211653.png)
![](images\cap_20171114_211715.png)
#### 1) Equation de movement

PFD: Principalement fondamental dynamique

$$\sum_{1}^nF_{\vec{p}->p}=m_{p}\vec{a_{p/Ro}}$$

On isole P(géométrie dimension nulle), ensuite (P+C)

~~~c
A=[0 1 0 0 0;0 0 40 0 10^-3;0 0 0 1 0;0 0 -5 0 -10^-4;0 0 0 0 -1];
B=[0;0;0;0;100];
C=[1 0 10 0 0];
D=[0];

sys=ss(A,B,C,D);
[num,den]=ss2tf(A,B,C,D);
T=tf(num,den);
roots(den);
Co=ctrb(sys); 
RCo= rank(Co);
Ob=obsv(sys);
ROb=rank(Ob);
%Poles dominants -0.2-0.2i -0.2+0.2i
P=[-1;-0.999;-1.001;-0.2-0.2i;-0.2+0.2i];
K=place(A,B,P);
Ar=A-B*K;
eig(Ar)

~~~

### TD04
![](images\cap_20171114_211520.png){:width="1000px"}
![](images\cap_20171114_211617.png){:width="1000px"}

### TD06
![](images\cap_20171114_211417.png){:width="1000px"}

!!! hint "5 équations"
		1 moteur:
		2 électromagnétique:
		1 mécanique:
		1 hydraulique:


~~~python
clc
clear

% parametres :
R_=1.36; L_=3.6e-3; Ke_=0.838; Ki_=0.838; Ka_=10;
b_=0.268; Kv_=0.5; S_=50; J_=1; Ks_=1.34;

C1_=(R_*b_+Ke_*Ki_)/(Ki_*Ka_);
C2_=(L_*b_+R_*J_)/(Ki_*Ka_);
C3_=(L_*J_)/(Ki_*Ka_);

A=[0 1 0 0; 0 0 1 0;0 -C1_/C3_ -C2_/C3_ 0; Kv_/S_ 0 0 -Ks_/S_];
B=[0;0;1/C3_;0];
%C=eye(4)
C=[0 0 0 1];
D=[0];
sys=ss(A,B,C,D);

Co=ctrb(sys);
nCo=rank(Co);
Ob=obsv(sys);
nOb=rank(Ob);

P1=[-0.15+0.15i, -0.15-0.15i, -1.5, -1.6];
K=place(A,B,P1)
L=place(A.',C.',P1).'
%Q=[25 0 0 0 0;0 1 0 0 0;0 0 100 0 0; 0 0 0 1 0; 0 0 0 0 0.01]
%rho=5
%R=[rho*(1/(50*50))]
%[K,P,E]=lqr(A,B,Q,R)
~~~

# Median 2016
![](images\cap_20171114_214218.png)

## 1) 微分方程
![](images\cap_20171114_214434.png)

~~~python
%初始化矩阵

A11=0;  A12=1;  A13=0;  A14=0;  A21=-k/jc;  A22=-lamc/jc;  A23=k/jc;  A24=0;  A31=0;  A32=0;  A33=0;  A34=1;  A41=k/jm;  A42=0;  A43=-k/jm;  A44=-lamm/jm;

A=[A11 A12 A13 A14;A21 A22 A23 A24;A31 A32 A33 A34;A41 A42 A43 A44];
B=[0;0;0;1/jm];
C=[1 0 0 0];
D=[0];
sys=ss(A,B,C,D);
[zero,pole,k_gain]=zpkdata(sys,'v'); %'v'返回多项式系数向量
~~~

## 2) 代入数据
![](images\cap_20171114_214539.png)

~~~python
%赋值操作初始化
k=0.8; jm=4/10000; jc=4/10000; lamm=0.015; lamc=0;
~~~

## 3) 可控性和可观性
![](images\cap_20171114_214615.png)

~~~python
%{可控与可观%}
Co=ctrb(sys); 
Rco= rank(Co);
Ob=obsv(sys);
Rob=rank(Ob);
~~~

## 4) Pôle dominant和性能指标
![](images\cap_20171114_214652.png)

~~~python
%{pole dominant%}
kesi=0.5; %Coef d'amortissement
tr=0.5; %temps de reponse
omega0mini=3/(kesi*tr);  
p1=-kesi*omega0mini+1i*omega0mini*sqrt(1-kesi^2);
p2=-kesi*omega0mini-1i*omega0mini*sqrt(1-kesi^2);
~~~

- poles dominants: dans un rapport 10 avec les valeurs reel pole dominant
  p=-$\xi$*$\omega$0$\underline{+}$j*$\omega$0*sqrt(1-$\xi$^2);
- les carateristiques:
	- ![](images\cap_20171114_174555.png)
	- ![](images\cap_20171114_180018.png)
	- ![](images\cap_20171114_175732.png)
	
!!! question "如果结果很奇怪，那就更改solver"
	![](images\cap_20171114_181247.png)

性能指标参数：

	Tr5%=0.475	tm10-90=0.144	D%=15.6	e0=0

## 5) 矩阵P,K和性能参数(重新计算K)
![](images\cap_20171114_214752.png)

~~~python
%{dans un rapport 10 avec les valeurs reel pole dominant %}
P=[p1; p2; 10*real(p1); 10*real(p2)+0.01];
%La matrice de contre reaction K 
K=place(A,B,P);
~~~

Couple maxi 
![](images\cap_20171114_182417.png)
	X1和Cm都符合要求

## 6) 矩阵L和观测器
![](images\cap_20171114_214838.png)

~~~matlab
%dans un rapport 5 pour pole d'observateur
Pob=[p1; p2; 5*real(p1); 5*real(p2)+0.01];
Kob=place(A,B,Pob);
L=place(A.',C.',Pob).';
~~~

Pole d'observateur: dans un rapport 5 pour pole d'observateur
Pob=[p1; p2; 5*real(p1); 5*real(p2)+0.01];
重新计算K
Kob=place(A,B,Pob);
L=place(A.',C.',Pob).';

## 7) 观测器和系统的积分部件的初始条件各自对X1和Cm的影响
![](images\cap_20171114_215102.png)
	
Des CIs pour
	- X1 ![](images\cap_20171114_185005.png)
	- Cm ![](images\cap_20171114_185256.png)

8. 计算Q,R矩阵
![](images\cap_20171114_215253.png)

计算 矩阵Q和R:

~~~python
%{LQR pour optimiser la matrice de gain%}
trsi=0.5;  %desired settle time (tr5%)
x1m=pi;
x2m=100;
x3m=10;
x4m=100;

Q11=1/(trsi*x1m^2);
Q22=1/(trsi*x2m^2);
Q33=1/(trsi*x3m^2);
Q44=1/(trsi*x4m^2);
Q=[Q11 0 0 0;0 Q22 0 0;0 0 Q33 0; 0 0 0 Q44];
u=0.5;
r1=1/u^2;
rho=1;
R=rho*r1;
~~~

## 9)权重$\rho$及其确定的Kopt和系统性能指标
![](images\cap_20171114_215331.png)
pour valoriser le parametre prépondérant X1, on prends $\rho$=0.6:

~~~python
rho=0.6;
[Kopt,Popt,E]=lqr(A,B,Q,R);
~~~

把K换成Kopt,
![](images\cap_20171114_193243.png)
重新模拟得到新的X1$_{maxi}$=1
![](images\cap_20171114_192751.png)

可得到如下性能指标参数：

	Tr5%=0.273	tm10-90=0.167	D%=0	e0=0

与之前相比	

	Tr5%=0.475	tm10-90=0.144	D%=15.6	e0=0

可得到结论：

- Eliminer le deplacement
- pas d'oscillation dans la zone transitoire
- Temps de montée reduit.

## 10)结合LQ和观测器
![](images\cap_20171114_215426.png)


## 完整matlab程序

~~~python
clc
clear

%{
传递函数初始化
1.已知多项式
den=conv([1 0 0],conv([1 3],[1,2,3,4]));

2.已知zpk
z=[0];p=[1,3,5];k=6;
sys=zpk(z,p,k);
[num,den]=zp2tf(z,p,k);
[A,B,C,D]=zp2ss(z,p,k);

3.C=eye(3)
[V,D]= eig(A-B*K)返回矩阵的特征值和特征向量(根)
%}

%====赋值操作初始化====
k=0.8;
jm=4/10000;
jc=4/10000;
lamm=0.015;
lamc=0;

%====初始化矩阵====

%{
A11=;
A12=;
A13=;
A14=;

A21=;
A22=;
A23=;
A24=;

A31=;
A32=;
A33=;
A34=;

A41=;
A42=;
A43=;
A44=;

B=[0;0;0;0];
%}

A11=0;  A12=1;  A13=0;  A14=0;  A21=-k/jc;  A22=-lamc/jc;  A23=k/jc;  A24=0;  A31=0;  A32=0;  A33=0;  A34=1;  A41=k/jm;  A42=0;  A43=-k/jm;  A44=-lamm/jm;

A=[A11 A12 A13 A14; A21 A22 A23 A24; A31 A32 A33 A34; A41 A42 A43 A44];
B=[0;0;0;1/jm];
C=eye(4); %[1 0 0 0];
D=[0];
sys=ss(A,B,C,D);
[zero,pole,k_gain]=zpkdata(sys,'v'); %'v'返回多项式系数向量


%{
====可控与可观====
rang(Co/Ob) soit n,Le parametre n etant le nombre de variables d'etat dans le vecteur d'etat X 
%}
Co=ctrb(sys); 
Rco= rank(Co);
Ob=obsv(sys);
Rob=rank(Ob);
%判断
n=size(A,1); %返回向量的行数
if Rco==n 
    display('systeme controlable') 
    else; 
    display('systeme non controlable') 
end
if Rob==n 
    display('systeme observable') 
    else; 
    display('systeme non observable') 
end

%{
====pole dominant====
此时一般就要求性能指标==>simulink模拟
Temps de reponse a 5%(Tr5%)==> setting time
Temps de montee (Tm10-90)==> rise time
Peak reponse de depassement ==>	peak ampititude, overshot,at time
Erreur de position ==> difference de final value
Coef amortissement ==> damping raito
pulsation propre ω(rad/s) ==> natural frequence f*2π
%}

kesi=0.5;     %Coef d'amortissement
tr=0.5;        % temps de reponse ==>setting time
omega0mini=3/(kesi*tr);  
p1=-kesi*omega0mini+1i*omega0mini*sqrt(1-kesi^2);
p2=-kesi*omega0mini-1i*omega0mini*sqrt(1-kesi^2);

%dans un rapport 10 avec les valeurs reel pole dominant
P=[p1; p2; 10*real(p1); 10*real(p2)+0.01];
%La matrice de contre reaction K 
K=place(A,B,P);

%dans un rapport 5 pour pole d'observateur
Pob=[p1; p2; 5*real(p1); 5*real(p2)+0.01];
Kob=place(A,B,Pob);
L=place(A.',C.',Pob).';


%{
======LQR======
pour optimiser la matrice de gain
替换simulink中的Kopt
%}
trsi=0.5;  %desired settle time (tr5%)
x1m=pi;
x2m=100;
x3m=10;
x4m=100;

Q11=1/(trsi*x1m^2);
Q22=1/(trsi*x2m^2);
Q33=1/(trsi*x3m^2);
Q44=1/(trsi*x4m^2);
Q=[Q11 0 0 0;0 Q22 0 0;0 0 Q33 0; 0 0 0 Q44];

u=0.5;
r1=1/u^2;
rho=1;
%rho=0.6; %要求高性能
R=rho*r1;
[Kopt,Popt,E]=lqr(A,B,Q,R);


%{
======卡尔曼滤波器======
M，W，V的值
把L的值替换为Kf
%}

M=[0.1 0 0 0; 0 0.1 0 0; 0 0 0.00132 0 ;0 0 0 0.1];
W=[0.1 0 0 0;0 0.1 0 0;0 0 0.1 0;0 0 0 0.1];
V=[0.1 0 0 0;0 0.01 0 0; 0 0 7.46 0;0 0 0 0.01];

[Kf,P,E]=lqe(A,M,C,W,V);

~~~


# Median 2017
![](images\cap_20171121_214720.png)

## 1.建模
![](images\cap_20171121_214854.png)

F(t)-kX(t)=$\frac{md^{2}X(t)}{dt}$

donc，
![](images\cap_20171128_210641.png)

## 2.代数
![](images\cap_20171122_132129.png)
![](images\cap_20171122_135430.png)
![](images\cap_20171128_210723.png)

## 3.能控性和能观性
![](images\cap_20171122_132203.png)

~~~c
Co=ctrb(sys); 
Rco= rank(Co);
Ob=obsv(sys);
Rob=rank(Ob);
%décider
n=size(A,1); %retourner le nombre de ligne
if Rco==n 
    display('systeme controlable') 
    else; 
    display('systeme non controlable') 
end
if Rob==n 
    display('systeme observable') 
    else; 
    display('systeme non observable') 
end
~~~
![](images\cap_20171128_210803.png)

## 4.pôle dominant 和 系统响应特征
![](images\cap_20171122_132244.png)

~~~c++
kesi=0.7;     %Coef d'amortissement
tr=0.5;        % temps de reponse ==>setting time
omega0mini=3/(kesi*tr);  
p1=-kesi*omega0mini+1i*omega0mini*sqrt(1-kesi^2);
p2=-kesi*omega0mini-1i*omega0mini*sqrt(1-kesi^2);
~~~
![](images\cap_20171128_210929.png)



## 5. Kpôle
![](images\cap_20171122_132431.png)

~~~c
P=[p1; p2];
%La matrice de contre reaction Kpole
Kpole=place(A,B,P);
e0=1-0.986; % = 0.014
~~~
![](images\cap_20171122_141335.png)
![](images\cap_20171122_140622.png)
![](images\cap_20171128_211004.png)


!!! question ""
	P的取值什么时候为 dans un rapport 10 avec les valeurs reel pole dominant ？？？

## 6.LQR、FTBF的pôle
![](images\cap_20171122_132856.png)

~~~c
%LQ
trsi=0.5;  %desired settle time (tr5%) maxi
x1m=1.05;
x2m=10;

Q11=1/(trsi*x1m^2);
Q22=1/(trsi*x2m^2);
Q=[Q11 0;0 Q22];

u=100; %Fmaxi=100N, donc rho comment?
r1=1/u^2;
rho=0.5;
%rho=0.6; %performance
R=rho*r1;
[Kopt,Popt,E]=lqr(A,B,Q,R);

% pole de FTBF = eig(A-B*Kopt)
[Pf,d]=eig(A-B*Kopt);

~~~
![](images\cap_20171128_211102.png)


- Q

!!! question ""
	已经知道u.maxi,如何计算rho  ????
	Pole de FTBF怎么算  ????
	计算L时，pôle的值什么时候取 dans un rapport 5 pour pole d'observateur

## 7. 观测器
![](images\cap_20171122_133034.png)

~~~python
%Observateur
Pob=[0.3; 1.5];
Kob=place(A,B,Pob);
L=place(A.',C.',Pob).';
~~~
![](images\cap_20171122_161208.png)
![](images\cap_20171128_211238.png)


## 8.初始条件的影响
![](images\cap_20171122_133056.png)
![](images\cap_20171122_162136.png)
![](images\cap_20171128_211305.png)

# Part B 离散信号处理

## 序
- 疑问
	- margin state
	- 波特图
	- 确定系数：r0 r1
	- 为什么要变3阶
	- p1 p2 p3怎么确定
	- lerreur de position, vitesse

- 连续信号的采样会导致信息丢失
	- 采样周期
	- 数值取整数 quantization
		- 缩小 step size
	- 数据延迟
		- 处理周期：测量，数字化，算法，处理
		- 延迟缩小了带宽
	-电脑可以减少信息损耗

- 基本结构
![](images\cap_20171220_145521.png)
- discrete controller：
	+ Z 转化，应用于比较方程
	+ S 转化，应用于微分方程
	+ Z domain 用来创建比较方程
- 创建 digital z domain controller 的三种方法：
![](images\cap_20171220_150306.png)
![](images\cap_20171220_150743.png)

- 缺陷：
	+ 数字化会使高频信号损失，同时丢失了phase margin的稳定性。因为0.1s的采样增加了系统的phase lag


- 数字化的具体方法
![](images\cap_20171220_151834.png)
- z变换不是全部内容
	+ 数字化的方法
		* 'zoh'
		* 'foh'
		* 'impulse'
		* 'tustin' 只转换而没有hold过程
		* 'matched' 只转换而没有hold过程
		* 如果采样时间足够小，那么几种方法差别不大
	+ impulse 更贴近Z转换的计算式 但是跟实际的函数差距比较大：两次采样数据之间没有ZOH,所以数据会回落。如果想要跟实际的数据贴近那就用ZOH
![](images\cap_20171220_154803.png)
![](images\cap_20171220_155000.png)

ZOH的数学证明:
![](images\cap_20171220_161121.png)
![](images\cap_20171220_161057.png)
![](images\cap_20171220_161527.png)
![](images\cap_20171220_161745.png)


注意：
![](images\cap_20171220_162818.png)
![](images\cap_20171220_163303.png)
![](images\cap_20171220_164242.png)




## I) Z 变换
Transformée en Z
### 1 定义和性质
- 1.1 Laplace ==> Z 变换
Passage de la transformée de Laplace à la transformée en Z
![](images\cap_20171220_174811.png)

- 1.2 计算规则和性质
Propriétés et règles de calcul
- 1.3 Z表格
Établissement d'une table de transformée en Z

### 2 P平面和Z平面的对应关系
Correspondance entre le plan p et le plan des z
![](images\cap_20171220_175321.png)

### 3 Inversion de la transformée en Z
#### 3.1 Généralités
#### 3.2 Méthodes analytiques
![](images\cap_20171220_175712.png)

#### 3.3 Méthode de l'équation aux différences (équation récurrente)
![](images\cap_20171220_180037.png)


## II) 采样系统理论 
Systèmes échantillonnés
### 1 信号采样
Signal échantillonné
#### 信号分类
Classification des signaux 
- 1 Signal analogique
在时间和幅度上都是连续的
Le signal existe à tout instant t et peut prendre un nombre infini de valeurs. Il est continu dans le temps et en amplitude.
- 2 Signal échantillonné discret.
在时间上是连续的，幅度上不连续
Il n'existe que pour des instants donnés. Il peut prendre un nombre fini de valeurs aux instants considérés. Il est discontinu dans le temps et continu en amplitude.
- 3 Signal quantifié 量化的
在时间上是不连续的，幅度上连续
Il existe à tout instant t et peut prendre un nombre fini de valeurs. Il est continu dans le temps et discontinu en amplitude.
- 4 Signal numérique
在时间和幅度上都是不连续的
Il n'existe que pour des instants donnés et peut prendre un nombre fini de valeurs. Il est discontinu dans le temps et discontinu en amplitude.
#### 1.1 Définitions

Le plus souvent l'échantillonnage est effectué à des instants équidistants : l'espace entre ces instants est appelé période d'échantillonnage.
![](images\cap_20171220_180851.png)

#### 1.2 Échantillonneur bloqueur
![](images\cap_20171220_183857.png)

#### 1.3 Échantillonnage réel
![](images\cap_20171220_184026.png)

#### 1.4 Transformée de Laplace d'un signal échantillonné
##### 1.4.1 Transformée de Laplace d'une impulsion à l'origine
![](images\cap_20171220_184423.png)

##### 1.4.2 Cas d'une impulsion décalée de Te
![](images\cap_20171220_184519.png)

##### 1.4.3Transformée de Laplace du signal échantillonné réel
![](images\cap_20171220_184647.png)
On constate que la transformée de Laplace du signal échantillonné réel dépend de la nature de l'échantillonneur  à  cause  du  terme  e$^{−p\Delta t}$ (ce  terme  est  un  retard  pur)  ainsi  que  la periode d'échantillonnage Te
#### 1.5 Choix de la fréquence d'échantillonnage

le théoèrme de Shannon: $F_{e} > 2f_{0}$
En pratique, on adopte: $5f_{0} < F_{e} < 25f_{0}$
![](images\cap_20171220_185815.png)

#### 1.6 Modélisation de la transformée de Laplace d'un signal échantillonné
#### 1.7 Reconstitution d'un signal échantillonné

En traitement du signal, un bloqueur est un dispositif logique utilisé pour la commande des procédés, dans le cas de signaux échantillonnés.Il existe plusieurs sortes de bloqueurs. Leur principe est de « recréer » du signal entre des valeurs discrètes fournies en entrée. Le plus simple est le bloqueur d'ordre 0 dont la sortie garde la même valeur jusqu'à l'arrivée d'une nouvelle valeur en entrée. L'ordre du bloqueur représente la complexité de la section de courbe située entre deux valeurs :

- pour un ordre 0, la représentation de la sortie donnera un histogramme en barres
- pour un ordre 1, on aura des segments reliant directement deux valeurs successives, soit une interpolation linéaire basique.
- pour un ordre 2, on obtiendra une parabole entre 2 valeurs
- pour un ordre n, on obtiendra une courbe d'ordre n entre 2 valeurs successives.

##### 1.7.1 Fonction de transfert d'ordre 0 (BOZ)
![](images\cap_20171220_190144.png)

##### 1.7.2 Réponse fréquentielle du BOZ
![](images\cap_20171220_190444.png)

Si nous tolèrons une erreur $\varepsilon$ sur le signal reconstitué, on montre que la fréquence d'échantillonnage doit vérifier la relation : 
$$Fe≥f_{0}*\frac{2.2}{\sqrt{\varepsilon}}$$

### 2 采样系统
Système échantillonné

- 2.1 Définitions
- 2.2 Fonction de transfert échantillonnée
![](images\cap_20171220_195459.png)

#### 2.3 Application
##### 2.3.1 Premier cas
![](images\cap_20171220_200006.png)

##### 2.3.2 Deuxième cas
![](images\cap_20171220_200628.png)

##### 2.3.3 Fonction de transfert d'un processus muni d'un BOZ
![](images\cap_20171220_201524.png)

### 3 Synthèse d'une fonction de transfert en z
#### 3.1 Mise en évidence de l'équation récurrente
![](images\cap_20171220_202359.png)

#### 3.2 Cas d'un système quelconque

Pour retrouver l'équation récurrente, il suffira d'agir selon l'algorithme suivant :

- [x] Mettre F(z) sous la forme d'un rapport de deux polynômes en $z^{−k}$ .
- [x] En déduire la relation qui lie S(z) à E(z).
- [x] Exprimer la relation de récurrence sachant que multiplier $z^{−1}$ revient à retarder d'une période
d'échantillonnage .


## III) Comportement des systèmes asservis
### 1 Stabilité
#### 1.1 Stabilité absolue

On rappelle qu'un système est stable, lorsque écarté de sa postion initiale, il tend à y revenir.
Décomposons S(Z)/Z en éléments simples, puis revenons à S(z), il vient :

$$\frac{S(z)}{z}=\frac{A_{1}}{z-z_{1}}+\frac{A_{2}}{z-z_{2}}+\frac{A_{3}}{z-z_{3}}+...+\frac{A_{n}}{z-z_{n}}$$
**$$n : ordre.du.système$$**
Donc,
$$S(z)=\frac{A_{1}z}{z-z_{1}}+\frac{A_{2}z}{z-z_{2}}+\frac{A_{3}z}{z-z_{1}}+...+\frac{A_{n}z}{z-z_{n}}$$

Tous les termes de S(z) sont de la forme $\frac{z}{z-\alpha}$. Et pour originaux des termes de la forme $e^{aKT_{e}}$, avec:
- a<0 si ∣$\alpha$∣<1
- a>0 si ∣$\alpha$∣>1
Donc,
![](images\cap_20171220_220736.png)

**FTBF à retour unitaire**
![](images\cap_20171220_221003.png)

**FTBF à retour Non-unitaire**
![](images\cap_20171220_221330.png)

!!! caution "系统稳定的条件"
	在1+FTBO(z)=0条件下求得的根，其绝对值小于1
	Dans ces conditions un système bouclé est stable si les racines de son équation caractéristique 1+FTBO(z)=0 ont, chacune, leur module inférieur à 1.

#### 1.2 Critère de Jury 朱利判据

**两个条件:**

- [ ] 当特征方程中的最高阶数n为
	+ 奇数：
		* f(`z=1`)>0
		* f(`z=-1`)<0
	+ 偶数：
		* f(`z=1`)>0
		* f(`z=-1`)>0
- [ ] 验证
	+ 建立Jury array a,b,c各有两行，反向重新写一遍。`n≥3时才有jury表格`
		* 将Z的系数$a_{n}$按照Z的阶数从低到高排列
		* $b_{k}$的值 = det($a_{0},a_{n-k};a_{n},a_{k}$) ==> $b_{k} = a_{0} a_{k} - a_{n} a_{n-k}$
		* $c_{k}$的值 = det($b_{0},b_{n-k-1};b_{n-1},b_{k}$) ==> $c_{k} = b_{0} b_{k} - b_{n-1} b_{n-k-1}$
		* 以此类推直到只剩下3项 $r_{0},r_{1},r_{2}$，行数为`2n-3`,
	+ $|a_{0}|<|a_{n}|$
	+ $|b_{0}|>|b_{n-1}|$
	+ $|c_{0}|>|c_{n-2}|$
	+ ...
	+ $|r_{0}|>|r_{2}|$

#### 1.3 Application
##### 1.3.1 Système d'ordre 1
- $D(z)=a_{1}z+a_{0}$
	+ $a_{1}+a_{0}>0$
	+ $-a_{1}+a_{0}<0$

##### 1.3.2 Système d'ordre 2
- $D(z)=a_{2}z^{2}+a^{1}z+a_{0}$
	+ $a_{2}+a_{1}+a_{0}>0$
	+ $a_{2}-a_{1}+a_{0}>0$
	+ $|a_{0}|<a_{2}$

##### 1.3.3 Système d'ordre 3
- $D(z)=a_{3}z^{3}+a_{2}z^{2}+a^{1}z+a_{0}$
	+ $a_{3}+a_{2}+a_{1}+a_{0}>0$
	+ $-a_{3}+a_{2}-a_{1}+a_{0}<0$
	+ $|a_{0}|<a_{3}$
	+ $|a_{0}^{2}-a_{3}^{2}|>|a_{0}a_{2}-a_{1}a_{3}|$


#### 1.4 Application à un système du premier ordre
![](images\cap_20171221_000520.png)
![](images\cap_20171221_000749.png)
![](images\cap_20171221_001154.png)

!!! hint "失稳"
	因为误差只在采样的时候才计算，所以增加了系统的不稳定性

#### 1.5 Correspondance plan p – plan z
![](images\cap_20171221_001606.png)

#### 1.6 Degré de stabilité : robustesse
![](images\cap_20171221_001845.png)

### 2 Précision statique
#### 2.1 无干扰情况
Précision en l'absence de perturbation
![](images\cap_20171221_145205.png)

Pour  obtenir  l'écart  en  régime permanent, il suffit d'appliquer le théorème de la valeur finale.
![](images\cap_20171221_145401.png)

La valeur de l'écart en régime permanent dépend de la forme du signal d'entrée et celle de la fonction de transfert en boucle ouverte. D'une manière générale, celle-ci s'écrit :
![](images\cap_20171221_145551.png)

##### 2.1.1 échelon 输入响应
**Réponse à un échelon**
![](images\cap_20171221_145851.png)


##### 2.1.2 rampe 输入响应
**Réponse à une rampe**
![](images\cap_20171221_150648.png)

##### 2.1.3 accélération 响应
**Réponse en accélération**
![](images\cap_20171221_150914.png)

##### 2.1.4 Bilan
![](images\cap_20171221_151100.png)


#### 2.2 有干扰
Précision en présence de perturbations
![](images\cap_20171221_151740.png)
![](images\cap_20171221_165334.png)
![](images\cap_20171221_165530.png)

#### 2.3 Conclusions
积分器的引入可以解决精度的问题。
Les résultats obtenus sont tout à fait comparables à ceux obtenus pour les systèmes continus : l'introduction d'un ou plusieurs intégrateurs permet de résoudre le problème de précision. Notons toutefois que la période d'échantillonnage intervient dans le calcul de l'erreur de trainage.

Mais  attention :  le  calcul  n'est  effectué  qu'aux  instants  d'échantillonnnage  et  en  régime permanent. Les exigences fixées sur le nombre d'intégrateurs vont influencer très certainement le régime transitoire. Si la période d'échantillonnage est mal choisie (trop longue), le réglage sera très mauvais malgré une erreur nulle aux instants d'échantillonnage.

!!! hint "注意"
	积分器会很大程度上影响 transitoire

## IV) 系统的数字化
systèmes échantillonnés
### 1 Introduction
- 优点:
	+ L'échantillonnage peut augmenter la sensibilité et la souplesse, par exemple, certains organes de puissance de processus industriels contrôlant des puissances considérables, peuvent être pilotées par des signaux numériques sans qu'il soit nécessaire à des chaînes d'amplification importantes et coûteuses.
	+ Le système échantillonné est cadencée par une horloge, et on visualisera son comportement qu'aux instants d'échantillonnage. De ce fait, le système échantillonne est plus « lent » que le système continue car on a systématiquement une période d'échantillonnage de retard, alors que les réactions de celui-ci sont instantanées.
	+ Si le système est piloté par une machine programmable, il sera facile de mémoriser les signaux et de garder aussi longtemps que l'on veut, sans perte de précision, pour les réutiliser au moment voulu.

L'horloge d'échantillonnage va être un élément essentiel du système : n'oublions pas en effet que la fonction de transfert en Z est modifiée quand on change la période d'échantillonnage Te. Le choix de sa période sera déterminant dans la conception d'un système échantillonné et on montre que si celle-ci est bien choisie, les systèmes échantillonnés ont un meilleur comportement que le système continu équivalent.

### 2 选择采样频率
Choix de la période d'échantillonnage
#### 2.1 Considérations pratiques
Le théorème de Shannon:
$$F_{e}=\frac{1}{T_{e}} > 2f_{0}$$
où $f_{0}$ est la plus haute des fréquences transmises par le processus, caractérisée par

- premier ordre: la constante de temps $\tau$
- second ordre: La pulsation propre $\omega _{N}$
- En pratique, on adopte: $5f_{0} < f_{e} < 25f_{0}$

#### 2.2 Application à un premier ordre
![](images\cap_20171221_175007.png)

#### 2.3 Application à un second ordre
![](images\cap_20171221_180329.png)

#### 2.4 Exemples de choix de période d'échantillonnage
![](images\cap_20171221_180411.png)


### 3 Modèle mathématique des systèmes échantillonnés
#### 3.1 Forme générale
##### 3.1.1 Système du premier ordre muni d'un BOZ
![](images\cap_20171221_181118.png)

##### 3.1.2 Système du second ordre muni d'un BOZ
![](images\cap_20171221_181502.png)

#### 3.2 intégrateur avec BOZ
##### 3.2.1 Fonction de transfert d'un intégrateur
![](images\cap_20171221_181734.png)

##### 3.2.2 Forme générale avec un intégrateur
Aussi la forme standard d'une fonction de transfert en z
![](images\cap_20171221_182439.png)

#### 3.3 Modèle échantillonné du premier ordre
![](images\cap_20171222_092057.png)

##### 3.3.1 Comportement statique
![](images\cap_20171222_092234.png)
	
	l'échantillonnage ne modifie pas l'amplitude des signaux.

##### 3.3.2 Comportement dynamique

- En régime cintinue
    - Le comportement dynamique d'un système continu est influencé par la constante de temps：$\tau$
    - Le pôle de la fonction de transfert: $P_{0}=-\frac{1}{\tau}$
    - Temps de réponse à 5%: $t_{r}$ = $3\tau$

- En régime échantillonné `F(z)`:
	+ Le pôle : $Z_{0}=e^{\frac{-T_{e}}{\tau}}=e^{P_{0}T_{e}}$
		* toujours `inférieur à 1`
		* lié à la période d'échantillonnage $T_{e}$
	+ les temps de réponse à un échelon:
		* on va revenir à l'équation de récurrence:
		* ![](images\cap_20171222_093619.png)
		* ![](images\cap_20171223_133442.png)


#### 3.4 Modèle échantillonné du second ordre

La transformée en Z utilisé le changement de variable en $z=e^{pT_{e}}$, ce qui implique obligatoirement que tous les pôles de la fonction de transfert F(p) se transforme en $z_{i}=e^{p_{i}T_{e}}$ 

##### 3.4.1 Cas où les pôles sont réels
![](images\cap_20171224_002246.png)

- Gain statique : $G(0)=\underset{Z\rightarrow1}{lim} F(z)$
- Comportement dynamique :
	+ les pôles $Z_{i}$ 越靠近1，对应的$P_{i}$越趋近原点(`grande constante de temps`) $\Rightarrow$ 系统反应越缓慢
	+ 反之 les pôles $Z_{i}$ 越靠近原点，系统反应越快、
	+ L'influence du zéro `零点的影响`：
		* si -1<z$_{0}$<0, le zéro change peu de chose.
		* Plus z$_{0}$ se rapproche des pôles, plus le temps de montée diminue. La réponse est plus nerveuse.
		* Si $z_{i}<z_{0}<1$, la réponse présente un dépassement, d'autant plus important que z$_{0}$ est proche de 1.


##### 3.4.2 Cas où les pôles sont complexes
![](images\cap_20171224_154246.png)


- Gain statique : $G(0)=\underset{Z\rightarrow1}{lim} F(z)$
- Comportement dynamique


## V) PID 采样控制
Commande par PID, approché échantillonné
### 1.Introduction

#### 1.1Choix de la période d'échantillonnage
$\frac{T_{p}}{10} < T_{e} < \frac{T_{p}}{2}$

#### 1.2Choix de la méthode de discrétisation
[Voir](## 序)

### 2.Discrétisation des correcteurs.

Pour calculer les fonctions de transfert des correcteurs PI, PID et PID filtré, ces fonctions de transfert en 'z' seront mise sous la forme du rapport de deux polynômes :
![](images\cap_20171214_103953.png)
#### 2.1Régulateur PI numérique
Le régulateur PI a pour transformée de Laplace :
![](images\cap_20171224_202529.png)

#### 2.2Régulateur PID numérique.
![](images\cap_20171224_203459.png)

!!! danger "不是最优结构"
	由于微分器的存在放大了噪声，所以这个结构不是最好的，需要加入PID滤波器

	Cette structure n'est pas la meilleure à cause de la dérivée qui amplifie le bruit dans les hautes fréquences. Dans ce cas, il est préférable d'utiliser un correcteur PID filtré. Cependant, à titre illustratif, nous donnons le calcul de la discrétisation de ce PID. 

#### 2.3 Régulateur PID filtré numérique.
![](images\cap_20171224_204611.png)

### 3.Discrétisation du processus

#### 3.1Préambule.

La structure des correcteurs PID avec un réglage par placement des pôles n'autorise que des processus du premier et second ordre. 

!!! hint ""
	带有放置极点位置的调节器的PID只能用在1阶和2阶过程上

#### 3.2Processus du premier ordre.
![](images\cap_20171224_210425.png)



#### 3.3Processus du second ordre
![](images\cap_20171224_211214.png)

!!! note "a1, a2, b1, b2"
	- a1, a2, b1, b2
		+ 1. les pôles complexes
		+ 2. les pôles réels

##### 3.3.1 Cas des pôles complexes
![](images\cap_20171224_213344.png)

##### 3.3.2 Cas des pôles complexes mais c=0
![](images\cap_20171224_213505.png)

##### 3.3.3 Cas des pôles réels
![](images\cap_20171224_214151.png)

### 4.Commande d'un premier ordre par un P.I.
La commande d'un premier ordre par un correcteur P.I donne un comportement en boucle fermée du second ordre.
![](images\cap_20171224_214816.png)

#### 4.1 Comportement en premier ordre.
![](images\cap_20171224_221803.png)

#### 4.2 Comportement en second ordre
![](images\cap_20171224_222740.png)

#### 4.3 Résumé du réglage d'un correcteur P.I
##### 4.3.1 Dynamique du premier ordre
![](images\cap_20171224_223500.png)


##### 4.3.2 Dynamique du second ordre.
![](images\cap_20171224_224348.png)

##### 4.3.3 
![](images\cap_20171224_224443.png)

### 5 Commande d'un second ordre par un PID filtré.

Ici, le correcteur est du second ordre. Si nous voulons maîtriser complètement le fonctionnement en boucle fermée, le processus devra être du même ordre.

Dans le cas d'un simplification entre les pôles et les zéros du correcteur, le comportement en boucle fermée sera régi par un second ordre.

Dans le cas contraire, nous aurons un quatrième ordre dont les quatre pôles peuvent être fixés. Ce  deuxième cas aboutissant à des relations lourdes à gérer, nous ne traiterons que la première proposition.

!!! hint ""
	- `processus` 和 `correcteur` 的阶数都为`2`


Le schéma-bloc de l'ensemble processus-correcteur est le suivant :

#### Comportement du second ordre
![](images\cap_20171224_235715.png)


### 6.Conclusion
![](images\cap_20171214_110711.png)

Les correcteurs P.I, P.I.D et P.ID filtré ont respectivement une structure fixe du premier ordre et du second ordre. Nous avons, à partir de ces deux structures, montré comment il était possible de régler un processus modélisé par un premier ou second ordre.

Pour un fonctionnement en boucle fermée par l'utilisateur, il est aisé de calculer les expressions des polynômes S(z) et R(z) nécessaires à l'établissement de l'équation récurrente de la commande.

Le  calcul  des  actions  Kp,  Ti,  Td  et  N  n'est  pas  indispensable  pour faire  la  synthèse  du correcteur, leurs calculs dépendront du choix des paramètres de réglages accessibles par l'utilisateur.

Il est possible, à ce niveau, de dévoyer le concept de réglage d'un PID et considérer finalement que les actions accessibles au régleur sont la pulsation propre et le coefficient d’amortissement du système en boucle fermée. Dans ce cas, à partir de la connaissance de ω0  et m, l'unité numérique déterminer R(z) et S(z) à l'aide des relations vues dans ce chapitre et déterminer ensuite l'équation récurrente de la commande.

L'intérêt  de  cette  approche  est  d'offir  au  régleur  un  moyen  de  réglage  simple  présentant seulement  deux actions. Le réglage du coefficient d'amortissement le  dépassement et la pulsation propre fixe le temps de réponse. Il est clair que, dans une approche classique, les quatre réglages Kp, Ti ; Td et N influent tous sur le temps de réponse et le dépassement.

Dans cette synthèsed en temps discret d'un correcteur de type P.ID, il possible que certains valeurs de réglages Kp ;Ti, Td et N soient négatives. Contrairement au cas continu, cela n'implique pas que le correcteur ne soit pas réglables. Vous pouvez for bien appliquer la commande. Il est clair cependant que ; dans ce cas, l'équivalence au continu perd, quelque peu, de sa consistance.

Un dernier point pint qu'il ne faut pas perdre de vu dans une approche en temps discret, c'est le temps qui est discrétisé et qu'il inutile de sur-échantillonner. Vous choisirez donc un pas de calcul le plus grand possible tout en veillant eu respect du théorème de Shannon.

Si vous faites ce choix d'échantillonnage correct, approximativement entre le dixième et la moitié de la constante de temps principale du processus, le calcul des actions du correcteur devra se faire impérativement avec une approche échantillonnée.

## VI) Les équations polynomiales

- 1.Synoptique de l'asservissement
![](images\cap_20171225_000349.png)

- 2.Factorisation de la la fonction de transfert
![](images\cap_20171225_000929.png)
举例
![](images\cap_20171225_004001.png)

- 3.Structure du correcteur numérique K(z)
![](images\cap_20171225_001804.png)

- 4.Expression de la FTBO(z)
![](images\cap_20171225_001912.png)

- 5.Expression de la FTBF(z)
![](images\cap_20171225_002216.png)

- 6.Expression de l'erreur en régime permanent
![](images\cap_20171225_002332.png)

- 7.总结

!!! hint "套路"
	- Problème: trouver les polynômes $\Theta(z)$ et $\Pi(z)$. Cela revient à résoudre l'équation Diophantienne.
		- On fixe le régime transitoire avec le polynôme $D_{BF}(z)=N^{-}(z)\Theta(z)+ D^{-}(z)S_{r}(z)\Pi(z)$ équation Diophantienne(connue).
		- On fixe le type d'erreur à annuler en régime permanent $S_{r}(z)$(connu).
		- On fixe de plus une valeur à l'erreur d'ordre immédiatement supérieur (possibilité).


### 举例
![](images\cap_20171225_003749.png)
![](images\cap_20171225_151755.png)

- 目的: 设计un correcteur polynômiale
- 要求 cahier des charges:
	+ dynamique fixée par un second ordre : 
		* `w1=1.4 rad/s`  
		* `m=0.8`
	+ Ep: erreur de position nulle - `réponse à un échelon`
	+ Ev: erreur de vitesse nulle - `réponsde à une rampe`
- 初始条件:
	+ Le processus analogique a pour caractéristiques:
		* `w0=0.8 rad/s`
		* `m=0.1`
		* gain statique `T0=1`
	+ Période échantillonnage `Te=0.72`

!!! hint "思路："
	- 构建processus的传递函数
	- 根据CdCs构建要求的传递函数
	- 通过线性方程求corresctur的参数
		+ déterminer les polynômes $\Theta$(z) et $\Pi$(z) de l'équation Diophantienne $D_{BF}(z)=N^{-}(z)\Theta(z)+ D^{-}(z)S_{r}(z)\Pi(z)$ 
		+ Erreur position nulle , il faut donc un intégrateur Sr=(1-z^-1)
		+ Erreur de vitesse = 0.5 $\Rightarrow$ ev = $\frac{D^{-}(1).Ky(1).PI(1)}{DBF(1)}$ `DBF(1)=sum(D1d)` pour une rampe `Ky(1)=Te`
	- 构建corresteur K(z)
		+ $K(z)=\frac{D^{+}(z)S_{m}(z)\Theta(z)}{N^{+}(z)S_{r}(z)\Pi(z)}$
		+ 其中:
			* $D^{+}(z): Processus的TF方程的分母 \Rightarrow$ Dstable=tf(F0d.den{1},1,Te)
			* $S_{m}(z)$: 有integrateur:$(1-z^{-1})$没:1
			* $\Theta(z)$: 求解线性方程所得的 $\Pi$ 值的集合 $\Rightarrow$ Theta=tf(theta,1,Te)
			* $N^{+}(z)$: Processus的TF方程的 `增益k`$\times$`[1 零点的相反数]` $\Rightarrow$ [z0,k0]=zero(F0d); Nstable=tf(1,k0*[1 -z0],Te) 

~~~matlab hl_lines="1 15 26 43"
%%%% Le processus analogique TFBO
Te=0.72; % Période échantillonnage
w0=0.8; % pulsation propre du processus en BO
m=0.1;   % coefficient d'amortissement
T0=1;   % gain statique

num0=[T0];
den0=[1/w0^2 2*m/w0 1];
F0=tf(num0,den0)    % F0(p)
F0d=c2d(F0,Te)      % discrétisation de la fonction de transfert en BO

% T0d=zpk(F0d);
% [z0d,p0d,k0]=zpkdata(T0d)

%%%% FTBF fixé par le cahier des charges
w1=1.4 ; % pulsation propre du processus en BF
m1=0.8;  % coefficient d'amortissement
num1=[1];
den1=[1/w1^2 2*m1/w1 1];
F1=tf(num1,den1)
F1d=c2d(F1,Te) % Discrétisation de la fonction de transfert en BF 
% [z1d,p1d,T1d]=zpkdata(F1d) % liste zéro - pôle - gain

[D1d]=F1d.den{1} % D1d=z^2+D1d(2)*z+D1d(3)

%%%% Coefs du correcteur
ev=0.5; % erreur de vitesse en régime permanent
DBF1=sum(D1d) % =DBF(1)
%% Résolution du système linéaire 
% 不存在积分器
A=[0 0 1 0;1 0 -1 1;0 1 0 -1;0 0 1 1]
% 存在一个积分器
% A=[1 0 1 0;0 1 -1 1;0 0 0 -1; 0 0 1 1];
B=[1;D1d(2);D1d(3);ev*DBF1/Te]
Sol=A\B % Solution du système
theta0=Sol(1);
theta1=Sol(2);
pI0=Sol(3);
pI1=Sol(4);
theta=[theta0 theta1]; %polynôme theta(z)=theta0*z+theta1
pI=[pI0 pI1]; %polunôme pI(z)=pI0*z+pI1

%%%% Réalisation du correcteur
%%% K(z)=[Dstable(z)Sm(z)Theta(z)]/[Nstable(z)Sr(z)Pi(z)] D和N交换位置
Ds=F0d.den{1} ;
%获取的系数需要再Z转换
Dstable=tf(Ds,1,Te)
Sm=1;	% pas d'integrateur
Theta=tf(theta,1,Te)

[z0,k0]=zero(F0d); %return zero point of sys F0d
Ns=k0*[1 -z0]; 
Nstable=tf(1,Ns,Te)% 1/N+(z)
Sr=[1 -1]; % un integrateur
tfSr=tf(1,Sr,Te)	% Sr(z)
Pi=tf(1,pI,Te) % 1/pI(z)

K=Dstable*Theta*Nstable*tfSr*Pi

%%%% Extraction simulink
NK=K.num{1}
DK=K.den{1}

NP=F0d.num{1}
DP=F0d.den{1}
~~~


## TD习题
### TD03

### TD04
![](images\cap_20171130_121908.png)

### TD05

- Objectifs   :
    - Modéliser un système.
    - Établir la fonction de transfert du système. 
    - Étudier différents correcteurs numériques. 
    - Utiliser les outils Matlab, pour mener à bien l'étude.
    - Utiliser les outils Simulink , pour mener à bien l'étude.

#### Ex01 Présentation du système :
Le  système  à  asservir  en  vitesse  est  moteur  à  courant  continu  dont  les  caractéristiques  sont  les suivantes :
![](images\cap_20171214_114459.png)

- R: résistance de l'induit
- L: inductance de l'induit (que nous négligerons)
- J: inertie ramenée sur l'arbre moteur
- T$_{r}$ : couple résistant exercé par la charge mécanique (ici égal à zéro) 
- K$_{e}$ : constante de vitesse du moteur
- K$_{I}$ : constante de couple du moteur
- f: frottement fluide (coéfficient de frottement visqueux)

建模

- [x] 1.Equation électrique:
	+ $V(t) = R.i(t) + L\frac{di(t)}{dt} + \overbrace{ K_{e}.\Omega(t)}^{Force.ElectroMotrice}$

- [x] 2.Equation mécanique:
	+ $J.\frac{d\Omega(t)}{dt} = \overbrace{K_{I}.i(t)}^{moment.couple.electromagnetique} - \overbrace{ f.\Omega(t)}^{Perte} - C_{charge}(t)$

[补充图片](images\cap_20171214_115623.png)
[补充解释](images\cap_20171214_115736.png)


计算步骤:
![](images\cap_20171214_121053.png)

~~~matlab
r=2;        % Résistance d induit S.I
k=0.1;      % constante de vitesse et de couple du moteur SI
f=0.2;      % frottement visqueux
j=0.02;     % Inertie de l arbre moteur
l=0.5;		% inductance de l induit (que nous négligerons)
~~~

#### Ex02 Fonction de transfert continue
![](images\cap_20171214_114939.png)

~~~matlab
TauxBO=(r*j)/(r*f+k^2);			% constante de temps du système
k0=k/(r*f+k^2);					% gain statique
w0=1/TauxBO;					% pulsation naturelle, boucle ouvert
frequence=w0/(2*pi);			% fréquence naturelle,w=2*pi*fréquence

num0=[k0];
den0=[TauxBO 1];
Tm=tf(num0,den0,'variable','p')    % Tm fonction de transfert continue
~~~

!!! note "Output"
	Tm = `0.2439 / (0.09756 p + 1)`

#### Ex03 Discrétisation du processus
La période d'échantillonnage du processus à discrétiser est Te choisie est de 50ms.

- 3.1 Justifier le choix de la période d'échantillonnage.
Le processus est discrétisé avec un échantillonneur d'ordre zéro.
![](images\cap_20171214_122039.png)


~~~matlab
Te=20e-3;			% pédiode d échantillonnag d après CdCh
Tmd=c2d(Tm,Te)		% Tmd fonction de transfert discrétisée

%la constante de temps du système sans correcteur en boucle ouvert
Tauxp = stepinfo(Tmd,'RiseTimeLimits',[0,0.63])
~~~

!!! note "Tauxp"
	RiseTime: 0.08
	SettlingTime: 0.4
	SettlingMin: 0.156391105998548
	SettlingMax: 0.243902416439551
	Overshoot: 0
	Undershoot: 0
	Peak: 0.243902416439551
	PeakTime: 1.58

- 3.2 Établir la fonction de transfert du moteur : 
![](images\cap_20171214_122711.png)

~~~matlab
zpk(Tmd,'v')			% Pour voir ou sont les poles ( 'v') est la car c est une output
b1=Tmd.num{1}(2);				% coefficient numérateur de Tmd
a1=T0d.den{1}(2);				% coefficient dénonominateur de Tmd
~~~

!!! note "Output"
	b1 = 0.0978058012558279
	a1 = -0.598996214851105

#### Ex04 Étude de différents correcteurs
![](images\cap_20171214_122831.png)

##### 4.1 Correcteur proportionnel : K (z)=K
- 4.1.1 Étudier la stabilité du correcteur proportionnel en fonction de son gain K.

~~~matlab
%pour trouver valeur l'encadrement de K (0<K<intersection entre ligne bleue et cercle)

sisotool(Tmd); % plus puissant
%rlocus(Tmd);
%zgrid;

%on choisit la valeur de K
K=12; 

%Fct de transfert BO ac gain proportionnel
Tmd_BOp=series(K,Tmd);	

%connection en boucle fermé
Tmd_BFp=feedback(Tmd_BOp,1);	
step(Tmd_BFp);		%afficher le step graphe
~~~

- 4.1.2 Tracer les lieux d’Evan dans le plan complexe z. Commenter. 

~~~matlab
Steady_timeP=stepinfo(Tmd_BFp,'SettlingTimeThreshold',0.05);		%le temps qu il faut le système soit stable

sisotool(Tmd_BFp);

%{
rlocus(Tmd_BFp);		%root locus, lieux d Evans
axis('equal');
grid;		%readable
[k,poles]=rlocfind(Tmd_BFp);	%trouver la valeur du point
sgrid(Zeta,frequence);
%}


~~~

!!! note "Commenter"
	Faut que le point reste dans le circle

##### 4.2 Correcteur PI :
![](images\cap_20171214_124530.png)
Nous choisissons comme comportement en boucle fermé un système du premier ordre avec une
constante de temps en boucle fermée de 24,4 ms.
![](images\cap_20171214_124615.png)

- 4.2.1 Déterminer la fonction de transfert discrétisée de T$_{BFPI}$(z)

- cahier des charges
	+ erreur position nulle
	+ constante de temps de 24.4 ms
	+ gain unitaire


~~~matlab
TauxPI=24.4e-3		% constante de temps en BF
k1=1;				% gain statique
num1=[k1];
den1=[TauxPI 1];
T1=tf(num1,den1,'variable','p');

T1d=c2d(T1,Te);		%Fonction de transfert discretisée
%hold on
~~~

- 4.2.2 Déterminer les coefficients du correcteur

~~~matlab
% gain statique du système désiré
%确定B1的第一种方法
Alpha=exp(-Te/TauxPI);
B1=1-Alpha;
%确定B1的第二种方法
B1=T1d.num{1}(2) 
A1=T1d.den{1}(2)

%确定系数
r0pi=B1/b1;
r1pi=r0pi*a1; 

numPI=[r0pi r1pi];
denPI=[1 -1];

%直接转换为Z变换函数
Kpi=tf(numPI,denPI,Te,'variable','z');	%Fonction de transfert du correcteur PI

~~~

!!! note "Output"
	Kpi = `(8.907z-5.335)/(z-1)`

- 4.2.3 Déterminer la réponse à échelon de le temps de réponse à 5%.

~~~matlab
%确定闭环转换函数
Tmd_BOpi=series(Kpi,Tmd);	%Fct de transfert BO ac gain PI
Tmd_BFpi=feedback(Tmd_BOpi,1);

step(T1,T1d,Tmd_BFpi);
Steady_timePI=stepinfo(Tmd_BFpi,'SettlingTimeThreshold',0.05)
~~~

!!! note "Output"
	RiseTime: 0.05
    SettlingTime: 0.1
	SettlingMin: 0.983400133996619
	SettlingMax: 1
	Overshoot: 2.22044604925031e-14
	Undershoot: 0
	Peak: 1
	PeakTime: 0.9

- 4.2.4 Déterminer l'erreur de position et l'erreur de vitesse. 



##### 4.3 Correcteur PID :
![](images\cap_20171214_154739.png)

Nous choisissons comme comportement en boucle fermé un système du second ordre avec une
pulsation naturelle (ou propre) $\omega_{1}$=1/$\tau_{BO}$ et de coefficient d'amortissement m=0,707.
![](images\cap_20171214_155151.png)

~~~matlab
%给定的条件
%Determination FT discrétisée d apres Cdc
w1=1/TauxBO
m=0.707; %damping ratio, taux d amortissement, overshoot

~~~

- 4.3.1 Déterminer la fonction de transfert discrétisée de T$_{BFPID}$(z)

~~~matlab
num3=[1]; % gain statique
den3=[(1/w1)^2 (2*m)/w1 1]

%Fonction de transfert en BF discrétisée
T2=tf(num3,den3,'variable','p');
T2d=c2d(T2,Te)

% Fonction de transfert en BF discrétisée Ordre 3
Td=tf(1,[1 -0.001],Te)
T3d=T2d*Td  % Fonction de transfert d'ordre 3
~~~

- 4.3.2 Déterminer les coefficients du correcteur

~~~matlab hl_lines="1"
[P2d]=T3d.den{1} % Tableau contenant les coefficients du dénominateur
p1=P2d(2)
p2=P2d(3)
p3=P2d(4)

%Calcul des coefficients du correcteur PID  
r0pid=(1-a1+p1)/b1       
r1pid=(p2+a1)/b1
r2pid=p3/b1

%Fonction transfert du correcteur PID
numPID=[r0pid r1pid r2pid];
denPID=[1 -1 0];
Kpid=tf(numPID,denPID,Te,'variable','z')
~~~


- 4.3.3 Déterminer la réponse à échelon de le temps de réponse à 5%.

~~~python hl_lines="2 5"
%Fonction transfert en BO Correcteur + Processus
Tm_BOpid=Kpid*Tmd;   % fonction de transfert en B0

%Fonction transfert en BF Correcteur + Processus
Tm_BFpid=feedback(Tm_BOpid,1);   % fonction de transfert en B0

step(Tmd,Tmd_BFp,Tmd_BFpi,T2,T2d,T3d,Tm_BFpid)   % réponse à un échelon
legend('Tmd','Tmd_BFp','Tmd_BFpi','T2','T2d','T3d','Tm_BFpid')

~~~

- 4.3.4 Déterminer l'erreur de position et l'erreur de vitesse.


~~~matlab hl_lines=""
NPID=Kpid.num{1} % numérateur Kpid
DPID=Kpid.den{1} % dénominateur Kpid

NP=Tmd.num{1}    % numérateur du système en BO
DP=Tmd.den{1}     % dénominateur du sytème en B0
~~~



TD06
1. 建立微分方程
![](images\cap_20171222_152831.png)
![](images\cap_20171222_155836.png)

3.1 数字化
![](images\cap_20171222_155940.png)

~~~matlab hl_lines="1 8 16"
% parmatres du moteur DC 
r=2;            % Rsistance d induit S.I
k=0.1;          % constante de vitesse et de couple du moteur SI
f=0.2;          % frottement visqueux
j= 0.02;        % Inertie de l arbre moteur
l=0.5; 			%inductance de l induit

% Fonction de tranfert  continue
T0=k/(r*f+k*k);				%gain statique
w0=sqrt((f*r+k*k)/(l*j));	%pulsation naturelle, boucle ouvert
m0=(l*f+r*j)/(r*f+k*k)*(w0/2);	%damping ratio, taux d amortissement
num0=[T0];
den0=[1/(w0*w0) 2*m0/w0 1];
Tm=tf(num0,den0,'variable','p');    % Tm fonction de transfert continue

% Fontion de transfert discrtise sans correcteur
Te=10e-3;			%Periode d'chantillonnage d'aprs CdC
Tmd=c2d(Tm,Te)		% Tmd fonction de transfert discrtise
Tauxp = stepinfo(Tmd,'RiseTimeLimits',[0,0.63])       %la constante de temps Tp du systme sans correcteur en boucle ouvert
zpk(Tmd,'v')		%Pour voir ou sont les poles ( 'v') est la car c'est une output
~~~



3.2
![](images\cap_20171222_160043.png)

~~~python hl_lines="1"
%Coefs de TF moteur
b1=Tmd.num{1}(2)
b2=Tmd.num{1}(3)
a1=Tmd.den{1}(2)
a2=Tmd.den{1}(3)
~~~

4.1
![](images\cap_20171222_160122.png)

~~~python hl_lines="1 14"
%Fontion de transfert discrtise avec correcteur P 
rlocus(Tmd);axis('equal');grid; % l'encadrement de K (0<K<intersectione): 0<gain<200
%[k,poles]=rlocfind(Tmd_BFp); %trouver la valeur du point
% sisotool(Tmd);

%on choisit la valeur de K
K=12; 

Tmd_B0p=series(K,Tmd);			%correcteur P en serie 
Tmd_BFp=feedback(Tmd_B0p,1);	%boucle ferme

step(Tmd_BFp);		%afficher le step graphe
Steady_timeP=stepinfo(Tmd_BFp,'SettlingTimeThreshold',0.05)

%lieux d Evans
sisotool(Tmd_BFp);
~~~


4.2
![](images\cap_20171222_160234.png)

~~~python hl_lines="1 9 18 23"
%correcteur PID filtre
w1=4*w0;	%pulsation de BF d aprs CdC
m1=0.707;	%damping ratio, taux d amortissement, overshoot
num1=[1];
den1=[(1/w1)^2 (2*m1)/w1 1];
T1=tf(num1,den1,'variable','p');
T1d=c2d(T1,Te);		%Determination T1d(z)

%Coefs du correcteir PID
[P]=T1d.den{1};
p1PID=P(2);
p2PID=P(3);
r0pid=(1+p1PID+p2PID)/(b1+b2);
r1pid=a1*r0pid;
r2pid=a2*r0pid;
s1=r0pid*b2-p2PID;

%Fonction de transfert du correcteur PID filtre
numPID=[r0pid r1pid r2pid];
denPID=conv([1 -1],[1 s1]);
Kpid=tf(numPID,denPID,Te,'variable','z');

%La reponse a 5%
Tmd_BOpid=series(Kpid,Tmd)
Tmd_BFpid=feedback(Tmd_BOpid,1)
Steady_timePID=stepinfo(Tmd_BFpid,'SettlingTimeThreshold',0.05)

step(T1,T1d,Tmd_BFpid)
~~~

5.
![](images\cap_20171222_160303.png)

~~~python hl_lines="1"
%待研究
num2=[1];
den2=[1 0];
Gamma_statique=tf(num2,den2,Te,'variable','z');

Ks=(Gamma_statique)/(Tmd*(1-Gamma_statique));	%FT de statique
                                      
T_statique_pile_bo=series(Ks,Tmd);
T_statique_pile_bf=feedback(T_statique_pile_bo,1);
step(T_statique_pile_bf,T1d,Tmd_BFpid);
Steady_timestatique=stepinfo(T_statique_pile_bf,'SettlingTimeThreshold',0.05);

%Erreur vitesse nulle
num3=[2 -1];
den3=[1 0 0];
Gamma_trainage=tf(num3,den3,Te,'variable','z');
Kt=(Gamma_trainage)/(Tmd*(1-Gamma_trainage)); %FT de tranage
~~~


