# 重复博弈沙盒 Repeated game sandbox



## 1. 重复博弈
众所周知的囚徒困境中，博弈双方可以选择“合作”或“背叛”。只有当双方足够信任对方时，才有可能相互合作来获得最大的共同利益。然而“自私的基因”会使得玩家有“背叛”对方的倾向：若对方选择“合作”，那么自己可以从中渔利；若对方也选择“背叛”，至少自己不会一无所得。不论对手是选择“合作”还是“背叛”，“背叛”对方总是可以对自己最有利。这体现了在非零和博弈中，个人做出理性选择却往往导致集体的非理性。

囚徒困境似乎再简单不过了，然而，一旦进行多轮简单博弈，策略就变得种类多样并且有趣了起来。从简单博弈衍生出来的最直接的两个策略是“永远合作”和“永远背叛”，我们称这两种玩家为“全球主义者”和“自私者”。前者以所有玩家的共同利益为最高目标，所以即使对方有可能背叛自己，仍总是会选择“合作”。后者坚信“人不为己，天诛地灭”，只关心自己的得分，所以总会打出“背叛”牌。这两个策略不管进行多少轮博弈，结果只有一个：好人吃亏。那么，有没有一个既可以与好人合作，又不会被坏人坑太惨的策略呢？心理学家和博弈学家阿纳托尔（Anatol Papoport）教授提出了一个相当高效的策略：以牙还牙（Tit for Tat）。采取该策略的玩家在第一次博弈中选择“合作”，而在以后的博弈中，会选择上一轮对方对待自己的方法。那么，当“以牙还牙”要与“全球主义者”进行10轮比赛时，由于对手总是选择合作，所以双方会持续10轮相互合作，共同拿到很高的分数。而当“以牙还牙”要与“自私者”比赛10次时，虽然第一次被“自私者”背叛，对方获利，自己一无所得，但在后续的比赛中，它会报复对手，因为自私者在上一轮选择了“背叛”自己。这样在后续9轮里会和“自私者”相互伤害而获得一样的较低分数，从而避免了“全球主义者”得0分的结局。“以牙还牙”被认为是最可靠的基本策略之一，除此之外还有很多更为复杂和有趣的策略。

如果我们采用循环赛的办法，让10个“全球主义者”、10个“自私者”和10个“以牙还牙”分别进行重复博弈，任意一对玩家（某个策略不仅与其他策略较量，也要和其余9个和自己一样玩家较量）都进行一次轮数为10的重复博弈，那么就可以根据某一种玩家的平均得分来评价其综合能力。例如在所举的例子里，“以牙还牙”会比其他两种策略平均得分更高，因为与“全球主义者”和“以牙还牙”类的玩家博弈时，会相互合作而得到较高分数，与“自私者”博弈时，会避免得0分。而“全球主义者”由于经常被“自私者”背叛，得分最低。“自私者”由于只能从10个“全球主义者”那里渔利到较高分数，但与其余的19个玩家都无法相互合作，从而拿不到高分。这样的结论似乎隐喻着“精明的好人活的长久”的道理：一味地行善或是一味地作恶都是不可取的。这个沙盒似乎更像是一个小社会了，那它能否用来模拟什么样的人（策略）混得更好呢？我们实现了理查.道金斯在《自私的基因》一书中提到的进化循环赛，即：每次循环赛都可以得出一个平均分最高的策略和一个平均分最低的策略，那么沙盒将最高分策略的数量加一，将最低分策略的数量减一，让这30个玩家再进行一次循环赛。以此类推，我们可以发现，经过若干轮循环赛后，“自私者”被消灭了！沙盒里只剩下了少量的“全球主义者”和大量的“以牙还牙”，并稳定在这个状态。理查.道金斯称这种通过多次循环赛可以成为社会上主流的策略为“进化稳定策略（ESS, Evolutionarily stable strategy）”。重复博弈沙盒为使用者探究在何种博弈规则下会产何种进化稳定策略提供了有力工具。

## 2. 编译和执行

重复博弈沙盒以Java编写，可以使用maven编译并运行jar包（已提供pom.xml文件），或由eclipse导入项目后编译运行（已提供.project文件）。

## 3. 沙盒接口

repeated-game-sandbox（rgs）包含三个包：sandbox、game和player。

### 3.1 game (package)

game包中的类用来定义所要进行的博弈的规则是什么。包含了阶段博弈接口IStageGame、阶段博弈抽象类AStageGame（实现了IStageGame接口）、收益矩阵类PayoffMatrix，以及继承于AStageGame的子类，如囚徒困境PrisonersDilemma。

我们希望此沙盒能够不仅仅用于模拟以囚徒困境为阶段博弈的重复博弈问题，也希望能够使其有能力扩展到其他更为复杂的博弈问题。因此，设计了阶段博弈接口IStageGame来规定一次基础博弈必须实现的行为，用其抽象子类AStageGame提供了一些常用方法，将需要实现什么博弈问题开放给rgs使用者。目前rgs仅实现了囚徒困境这一种博弈问题。

+ **IStageGame** (interface)

	IStageGame是定义了阶段博弈的接口。一个阶段博弈

    public int actionDimension(); // return the max num of action: 0, 1, n-1

    public int[] getScores(int a1, int a2); // return the scores of playerA and playerB if they took actionA and actionB

    public PayoffMatrix getPayoffMatrix(); // return the payoff matrix

    public int generousAction(); // return the action that benifits all

	public int selfishAction(); // return the action that benifits oneself

+ **AStageGame** (abstract class)

	实现了IStageGame接口。

+ **PayoffMatrix** (class)

已实现的阶段博弈类：

+ **PrisonersDilemma (囚徒困境)**

	继承于AStageGame抽象类。

### 3.2 player (package)

+ **IPlayer** (interface)

+ **APlayer** (abstract class)

	实现了IPlayer接口。

已实现的玩家类：

+ **Globalist (全球主义者)**：“为全人类的共同利益而奋斗！”

	继承于APlayer抽象类。

+ **Selfishman (自私者)**：“人不为己，天诛地灭”

	继承于APlayer抽象类。

+ **TitForTat (以牙还牙)**：“以牙还牙，以眼还眼”

	继承于APlayer抽象类。

+ **Idiot (傻逼)**：“@$%^&!@@*#&????”

	继承于APlayer抽象类。

+ **Recognizer (识别者)**：“我是卖木梳的——同志，我可找到你了！”

	继承于APlayer抽象类。

### 3.3 sandbox (package)

## 4. 示例说明

