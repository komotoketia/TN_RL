# TensorNetwork + ReinforcementLearning
テンソルネットワークは元々は統計物理で用いられる数値計算を効率化したりする手法ですが、近年ではデータ圧縮やニューラルネットワークの重み行列の圧縮などにも用いられています。

私はこのテンソルネットワークは強化学習に適用して効果があるのかを研究しています。
深層強化学習で用いられるDeepQNetworkにおけるニューラルネットワークの代わりにテンソルネットワークを使用したAIモデルでタスクを実行できるかを検証します。

## タスク
4*4のマップ上の固定位置(例えば下の動画なら左から１マス,上から２マス)に青い敵ロボットがあり、マップ上のランダムな位置に生成された白いロボット(Agentと呼ぶ)は上下左右斜めに１マスずつ動き、敵ロボットに隣接するマスに着いたら敵ロボットに攻撃する(敵ロボットは攻撃されたら赤くなる)。敵ロボットを攻撃したらそのepisodeは終了し、Agentはまたランダムな位置に生成される。

![タスク](https://github.com/komotoketia/TN_RL/assets/49736231/bd8b1e66-4170-499e-8f57-004a23eabf51)
![タスク2](https://github.com/komotoketia/TN_RL/assets/49736231/c856dc09-6681-40f2-a705-ac9abcb41345)


## 学習の様子

敵ロボットを攻撃できたらAgentは報酬を得ることができ、学習初期は同じ場所を行ったり来たりしているが、学習が進むと敵ロボットを攻撃するようになる。


下の動画では10個の4*4マップで並列に学習させることで学習の効率化を行っている。
各マップで確率 $\epsilon\ (0\leq \epsilon \leq 1)$をそれぞれ設定し、確率 $1-\epsilon$ で現在学習している行動確率における適切な行動を選択するようになる。(greedy法)
左上のマップでは $\epsilon=1$ となっておりAgentは完全ランダムに動く。右下のマップになるにつれて $\epsilon$ の値を下げていき、Agentはより高い確率で学習した行動確率で動くようになる。
一番右下のマップ では$\epsilon=0$ となっており、このマップの経過を見ればAgentの学習過程が分かる。

#### 学習初期
Agentは同じ場所を往復している

https://github.com/komotoketia/TN_RL/assets/49736231/f1302ba2-8634-49f5-89d3-3ac41f8f1953

#### 学習後期
Agentは敵ロボットを攻撃するように動くようになる

https://github.com/komotoketia/TN_RL/assets/49736231/2fd9038e-1f8e-4292-b199-72452c55c574
