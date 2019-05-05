# **第三章 PoS运行原理**

本章，我尝试描述PoS共识运行的步骤，来了解其大致的原理。

一般情况下，PoS共识的运行有7个基本步骤，运行节点，注册成验证人，Stake，选举验证人，打包交易，广播交易，验证人确认，以下我会对每个步骤进行解释。

因为不同项目有不同的实现方案，当我们在看具体项目时，会发现有的项目会有细节上的不同，或者已有步骤的先后次序不一样，这些不同都是这些项目的特色，或是增加安全，或是提高性能。我把各个项目中的精华拿出来，我发现大致过程是差不多的，用一句话将PoS共识串起来：

**持币人将币Stake，获得出块权利，在指定时间打包交易，并广播出去，得到验证后，新区块生成。**

## 1.1. **运行节点**

持币人成为验证人之前，需要运行节点客户端，成为一个区块链分布式网络中的接入点，也叫节点。早期的区块链项目，为了方便大家接入网络，都会有命令行，为用户体验着想的团队还会开发一个简单的可视化客户端，即钱包客户端。这些钱包客户端以桌面版为主，一般支持Windows，MacOS和Linux3种系统，如Bitcoin Core，Parity等。客户端里集成了运行的命令，并且都是可视化的简单操作，用户只需要简单点几下，即可以运行起节点。

这个节点就是我们所说的，区块链分布式账簿中的一个点，这个点（也就是你运行节点的电脑），存储着区块链所有的交易记录，并将参与到整个网络的共识当中去。所以，你的电脑即充当了存储的功能，也充当着计算的功能。

那Nuls项目举例，首先在官网下载对应的钱包客户端，以MACOS为例

 ![Image text](https://tezos.oss-cn-beijing.aliyuncs.com/pic/pos-book/%E5%9B%BE%E7%89%87%205.png)

安装好钱包后，打开，进入区块同步的阶段。从途中可以见到主网高度为1987877个块，本地同步了374个块，按照网路速度和数据大小，区块同步可能持续的时间较长。

 ![Image text](https://tezos.oss-cn-beijing.aliyuncs.com/pic/pos-book/%E5%9B%BE%E7%89%87%206.png)

区块同步完成之后，你可以选择创建一个钱包，或者导入钱包

 ![Image text](https://tezos.oss-cn-beijing.aliyuncs.com/pic/pos-book/%E5%9B%BE%E7%89%87%207.png)
创建完成后的一个钱包

 ![Image text](https://tezos.oss-cn-beijing.aliyuncs.com/pic/pos-book/%E5%9B%BE%E7%89%87%28.png)
钱包创建完成，可以参与到PoS共识，即PoS挖矿中去。按照Nuls的共识，你可以将你的挖矿权益委托给矿工。钱包中已经列举了一些矿工，选择一个矿工，输入你要委托的Nuls数量，就可以参与到共识挖矿中来。

 ![Image text](https://tezos.oss-cn-beijing.aliyuncs.com/pic/pos-book/%E5%9B%BE%E7%89%87%209.png)
 ![Image text](https://tezos.oss-cn-beijing.aliyuncs.com/pic/pos-book/%E5%9B%BE%E7%89%87%2010.png)

除了钱包客户端之外，项目提供最多的方式就是命令行了，有编程基础的小伙伴，可以按照官方的介绍，执行相关的命令就可以了。命令行我们拿Tezso来举例一下，以下是用命令行创建Teozs节点的步骤：[https://github](https://github.com/tezoscommunity/FAQ/blob/master/Compile_Mainnet.md)[.c](https://github.com/tezoscommunity/FAQ/blob/master/Compile_Mainnet.md)[om/tez](https://github.com/tezoscommunity/FAQ/blob/master/Compile_Mainnet.md)[osc](https://github.com/tezoscommunity/FAQ/blob/master/Compile_Mainnet.md)[omm](https://github.com/tezoscommunity/FAQ/blob/master/Compile_Mainnet.md)[unity/F](https://github.com/tezoscommunity/FAQ/blob/master/Compile_Mainnet.md)[AQ/blob/master/Compile\_Mainnet.md](https://github.com/tezoscommunity/FAQ/blob/master/Compile_Mainnet.md)

登录Debian或是Ubuntu系统，并升级基础包（用你机器的IP将下面命令中的 &quot;192.155.xxx.xxx&quot; 替换掉）

ssh root@192.155.xxx.xxx

apt-get update

apt-get dist-upgrade -y

创建一个Tezos账户，不要以root权限运行服务。注意make build-deps这个命令需要sudo的权限

adduser tezos

adduser tezos sudo

reboot

ssh tezos@192.155.xxx.xxx

如果你运行的是Ubuntu 16, 执行下面的命令安装 bubblewrap和最新版本的git

sudo add-apt-repository ppa:ansible/bubblewrap

sudo add-apt-repository ppa:git-core/ppa

sudo apt-get update

创建环境初始化

sudo apt-get install -y patch unzip make gcc m4 git g++ aspcud bubblewrap curl bzip2 rsync libev-dev libgmp-dev pkg-config libhidapi-dev

安装OPAM

-
  - 方法一

sh \&lt;(curl -sL [ht](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[tps://r](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[aw](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[.githubusercontent.com/oc](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[a](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[ml](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[/opam/master/she](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[l](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[l/inst](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))[all.sh)](https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh))

-
  - 方法二

wget [https://githu](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)[b.com/ocaml/](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)[opam/releases/](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)[d](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)[ownload/2.0.0/opam-2.0.](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)[0-x86](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)[\_6](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)[4-linux](https://github.com/ocaml/opam/releases/download/2.0.0/opam-2.0.0-x86_64-linux)

sudo mv opam-2.0.0-x86\_64-linux /usr/local/bin/opam

sudo chmod a+x /usr/local/bin/opam

获得主网源码

git clone -b mainnet [h](https://gitlab.com/tezos/tezos.git)[ttps://gitlab.com/](https://gitlab.com/tezos/tezos.git)[t](https://gitlab.com/tezos/tezos.git)[ezos/t](https://gitlab.com/tezos/tezos.git)[e](https://gitlab.com/tezos/tezos.git)[zos.git](https://gitlab.com/tezos/tezos.git)

cd tezos

初始化依赖工具

make build-deps

编译

eval $(opam env)

make

设点节点身份

./tezos-node identity generate 26.

运行节点，等待节点同步成功

nohup ./tezos-node run --rpc-addr 127.0.0.1:8732 &amp;

不过在此要说明的一点就是，自己运行全节点，意味着你需要付出诸如存储空间，计算力，电力等资源，并且保证你是24小时开机，才能让一个完整的全节点钱包运行下去，同时可以获得PoS共识中Stake的收益，所以这看上去不是一个很简单的差事。

目前为止，全节点运行已经慢慢发展了几个阶段，下面我来说说这几个阶段的演变过程。

一般自己运行运行起来的节点是全节点，全节点就是记录了主网上线以来的全部数据。初期的PoS项目基本都是类似Nul这样的全节点钱包客户端，其模拟的也是比特币初期可用电脑CPU挖矿的原理，运行节点的门槛也就是需要一台计算机。但全节点有个不好的地方，随着区块数据的增多，会需要更多存储空间。同时，钱包客户端不能掉线，也就是不能关机，断网或者断电，要不然你无法运行节点来获得出块机会，已获得的出块机会在此种情况下也无法继续。

家用电脑不同于专业挖矿，所以随着个人全节点的缺点暴露的越来越多，开始出现专业运行的矿池，矿池是专业挖矿的机构，可以支持24小时，不断电，不停网的挖矿方式，普通用户如果不想自己守着电脑挖矿，可以将币 **发送** 给矿池，让矿池来帮助自己出块获得奖励。矿池通过收取一定的手续费来获得收入。

这也是委托的第一种方式，但是这种委托需要把币给到矿池手中，用户往往需要承担很大的风险，如果矿池失窃，或是卷款潜逃，用户往往一点办法都没有，需要极大的信任，还需要矿池维护人员有良好的职业操守。

项目方在运行一段时间后，也会发现个人Stak的意愿并不是很高，而自建矿池或者集成已有矿池到委托验证人的选择里面来，既可以帮助矿池做安全背书，也能大大提高个人Stake的意愿，维护网络安全（当然，前提是对验证人有严格的审核），所以我们开始看到第二种委托形式——项目方支持委托权益的转移了。这种形式下，个人只需要将币转移到中间账户，或者是发送个合约账户，就可以完成奖励的获得，承担的风险大大降低。

但第二种形式的委托也有问题，毕竟用户无法使用Stake的币，大量用户会因为流动性原因不进行Stake。后来有项目方做了进一步的妥协，支持委托只是建立一种关系，而不需转移代币，Tezos上的委托就是如此。用户发起委托，委托期间，但是仍然能支配自己的代币，极大的降低了非自愿运行节点用户Staking的成本（当然，第三种形式的委托，用户能自由支配委托代币是因为有节点帮助用户交了抵押金）。

但是，代交抵押金来出块的方式，并不能让节点获得足够大的收益，也不能以此形成更专业化的操作矿池，虽然对去中心化有极大的帮助，但是在一定程度上会降低一些节点验证人的动力。于是，第四种委托方式出现了，用户委托仍然是层关系，委托期间，Staking代币的归属权仍然属于用户自己，但是要使用，比如发送，需要解锁Stak的代币才可以，为了防范长程攻击，解锁的时间一般持续20天~40天左右，以提高攻击成本。

无论那种形式的委托，只要你想成为验证人，那么第一步肯定还是需要自己运行节点的。现在只要是公链项目都支持运行节点，大多数都提供了命令行的运行方式，钱包客户端运行的全节点已经开始慢慢退出舞台。运行节点变成了一件更加专业的事情，每个PoS共识项目都有自己的命令，按照命令执行即可以运行全节点。

## 1.2. **声明为验证人**

节点运行起来后，需要执行变成验证人的命令，执行完命令，就可以变成验证人。一般对验证人的要求比全节点的要求要多。首先你需要保证的就是验证人节点的配置符合项目的要求，一般情况下，验证人的配置要求要比全节点的配置要求要高。其次确认项目对验证人节点的一些硬性门槛，如持币量，锁定时间等。Teozs要求验证节点的持币量需要达到10000个XTZ，Cosmos要求验证节点的Stake量在前100位，Cardano要求节点是矿池，持币量（包含委托量）达到总体币量的1%。

除了硬性要求，还有一些软性实力的要求，比如验证人需要时团队，或者公司，要求不能用云服务器，要求在社区有一定的声誉，要求投票需要达到一定的数量等等，这些门槛都需要在申请验证人之前了解，如果在不了解的情况下运行，可能无法声明成为验证人。

同样以Tezos为例，Tezos对机器配置的要求不高，但有持币10000个XTZ数量的门槛，达到门槛，执行以下命令成为验证人

激活成为Baker

./tezos-client add address my\_account \&lt;public key hash from donation PDF\&gt;

./tezos-client activate fundraiser account my\_account with \&lt;activation key from verification site\&gt;


## 1.3. **质押Stake**

Stake和成为验证人可以合成一部，由于Stake在PoS共识中的重要性，我把这两部拆开了。成为验证人之后，该验证人账户中的币会被检测，带有门槛的PoS共识会在此检测一些条件，当条件符合时，账户被标识为合格，然后放到验证人列表当中。如果不合格，此验证人并不会在真正的验证人列表里面。

合格的验证人，其账户里面所有的币都是质押成为Staking的币，系统会计算该账户的Staking数量在总体Staking量中的占比，使用算法来选举每个区块的验证人。

在Tezos上没有Stake的操作命令，只需要生命成为验证人的账号有代币，成为验证人之后，该账户的币就相当于默认Stake了。另外，需要注意的是，在Tezos上执行成为验证人的命令时，系统就会进行验证人门槛的判断，比如账户中是否有10000个XTZ。只有满足门槛后，才能成为验证人，并且进行Stake。


## 1.4. **选举验证人**

系统会从候选的验证人列表当中选择出块人。选择出块人的算法有很多种，每种的优缺点都不一样，但是它们都有一个共同要保证的任务，那就是保证选举结果不能被操纵，不能被预测，防止Grind Attack的攻击发生。只有安全性足够高的算法，才能保证整个token的激励系统不会被单一方所控制。

以下我们来列举几种目前比较流行的选举方式：

**Follow-the-Satoshi**

Follow-the-Satoshi算法，最早出现在论文《[Proof of Activity: Extending Bitcoin&#39;s Proof](https://eprint.iacr.org/2014/452.pdf)[o](https://eprint.iacr.org/2014/452.pdf)[f](https://eprint.iacr.org/2014/452.pdf)[Work via Proof of Stake](https://eprint.iacr.org/2014/452.pdf)》中，其作用就是在PoA（Proof of Activity）共识当中选出出块人。

Satoshi这里的意思是聪，是比特币电子货币形式里，最小的计数单位，每个已被铸造出来的聪都对应一个UTXO（未经花费的输出--Unspent Transaction Output），而每个UTXO都可以对应到一个私钥，而每一个私钥都可以对应到一个持有人。如果用持币权重（Voting Power）代替算力权重的话，PoS共识中的持币人其权重，其实就是多个聪的集合，所以选择出块人，即从这些聪的集合当中选择就可以了，哪个持有人中拥有最多被选中的聪，那他就是这个高度的出块人。这就是Follow-the-Satoshi的原理。

PoS中的持币人staking后，可以被最小单位s划分成很多份，Follow-the-Satoshi当中会有一个随机数输入源p，充当最开始的随机起始点，然后随机选出这个出块人。每个持币人都有多份s，那么单个块的出块人就是当前包含最多p的持币人。

运用Follow-the-Satoshi算法的PoS项目有Tezos和Cardano。

Tezos对算法做了改良，因为聪的单位太小，随着系统锻造出来的聪越来越多，选择算法会变得低效，所以Tezos进行了规整，以10000XTZ为一个卷（Roll），将卷作为随机数选择的目标，以此来提高选择效率，包含选中最多的卷将成为块的出块人。同时，为了让随机数源变得不可预测，Tezos将4096个块当成一个选举周期（Cycle），利用上一个周期出块人披露的Nonce（在通信安全中，Nonce是一个在加密通信只能使用一次的数字）为随机数输入源。未披露的验证人将会被扣除抵押金。

Cardano的做法和Tezos类似，以一定区块数为选举周期（Epoch），每个周期有一个创世块，每个创世块中包含了每个块的出块人和一个随机种子p。和Tezos不同的是，Cardano每个块只有一个出块人，如果这个出块人掉线，这个块会被抛弃，直接进入下一个区块。而Tezos中单个块会有多个出块人，这些出块人会有不同的优先级，以避免备选出块人出现的种种情况。

**Round Robin**

维基百科对Round Robin的解释是：

术语循环/轮转/轮替（英语：Round-robin）用于多种情况中，通常指将多个某物轮流用于某事，例如&quot;逐户派对&quot;（round-robin-party）中所有参与者要挨家挨户地拜访每位参与者的住处并参加那里的小型聚会。联名信（round-robin letter）往往是指一大群下属为批评其领导而写的一封信，这种信一般只在签名人数多到难于逐个报复后才会寄出。

一个简单的Round Robin算法的例子如：F(i, b) = 1 if b.height % N == i else 0

RR（Round Robin）常用于联盟链或者私有链居多，因为循环出块的可预知性太强，如果在一个可允许被随意进入（Pemissionless）的区块链网络里面，被攻击的概率会比较大，系统抗风险性变差。所以，在联盟链或者私有链的网络里面，各个节点互相知道/审核/同意（Pemission），利用RR既是一种简单可行的方式，又是一种高效的算法。

但是，后来有项目方改良了RR的算法，用于到Pemissionless的区块链网络中来确定出块人。我不确定是否是EXONUM团队对RR进行了首次改良，但是我在google找到到了他们改良的[方案](https://exonum.com/blog/04-28-17-leader-election-roundrobin/)。相对的，在Multichain的[白皮书](https://www.multichain.com/download/MultiChain-White-Paper.pdf)中也有将RR算法运用到联盟链中的说明。

在EXONUM里面，RR的执行逻辑是，在一个高度H里面，所有出块人排列成一定的顺序（优先级），如果首位出块人掉线则直接跳过，第二位出块人代替首位出块人位置来出块，如果第二位出块人掉线，则排在第三位的出块人顶上，以此类推，由此得出循环算法的说法。在高度H+1里面，在H高度的出块人将不在获得出块机会，其出块权利会被锁定一定的周期F，只有等权利解锁后才会再次被加入的排序中来。

当然，每个高度H的出块人排序不能是固定的，那么会被攻击者找到攻击目标。所以每个高度H的顺序由算法得出，T = Hash(H) mod M!。可以确定每轮（Round），验证人的排序都不一样，这样就避免了相同顺序下，验证人合谋的低成本问题。

Cosmos同样使用了RR算法来进行验证人选举。Cosmos按照出块人的权重（Voting Power），来确定高度H下的出块人（Proposer），每个高度的第一个Proposer对出块进行提议，然后进入该高度H的第一个Round，其他验证人对该提议进行两轮投票（Prevote和Precommit），两轮都超过2/3通过，该区块被commit，成为高度为H+1的块。如果中间的任意议论投票中，有一轮的投票并没有达到2/3的权重（Voting Power），那么第一个Proposer的提议被否决，这进入第二个Proposer的提议阶段，提议重复第一个Proposer提议的流程，以此类推。

在写此书的时候，Cosmos的测试跑到了Gaia9003，Genki4002，同时还进行着一个主网上线前的测试网网络Game of Stake，对于测试网中的很多功能还在测试，包括验证人选举，所以对于RR的算法并不是最终版本，等到主网上线后，我会再来补充具体的选举出块人方式。

**DPoS**

DPoS是由Daniel Lamier在2013年的比特股（Bitshares）上提出来的，其底层是基于石墨烯平台来搭建，我在此处说明的是基于石墨烯底层搭建的DPoS系统，其他的委托权益证明共识Delegate-PoS不在此范围之内。

基于石墨烯的DPoS，其出块人选举和RR算法有点像，更像是简单版本的RR。在DPoS的系统中，所有出块人由用户投票得出，投票最高的几位出块人获得出块机会。通常，出块人的数量是固定的，如EOS里面固定是21位，这21位出块人被投票出来之后，在一轮（126个块，每个出块人6个块）中，其出块顺序由15个及以上的生产者约定的顺序安排。

目前EOS的约定顺序是按照字母a-z排列的，比如Asia会比Basic排在前面出块，而Canada会在Basic后面出块。目前的这种排序方式，决定了每轮的排序其实有一样的顺序。

所以DPoS里面一个很基础的选举方式就是，系统规定一个固定数量的出块人，出块人由持币人投票得出，投票率按照高到低排序，系统按照高到低选择指定数量的出块人，然后将出块人随机排序，随机源可能是时间戳，也可以能出块人之间随机的参数。

**多方计算**** （multiparty computation (MPC) ）**

多方计算是由Cardano提出来的，目的是保证选举算法中的无偏差性，?通过计算获得一个随机数，作为选举算法的输入。随机源计算出来后，选举算法仍然使用的是3.4.1 follow the satoshi的方法。为了避免重复，我们这里说一下MPC。

在Cardano的文档中，对多方计算有以下的解释：

多方计算（multiparty computation (MPC) ）方法用来实现选举的随机性，每个参选人独立进行一次『投硬币』的行为，然后与其他参选人分享结果。这个想法就是：结果由每个参选人随机产生，但最终它们在相同的最终价值上达成一致。

Cadano使用的是类似于扔硬币的方案来决定区块人，分为3个阶段。提交，开启和恢复阶段。在提交阶段，参选人通过私钥签名一个提交，并附上周期标号，公钥等信息，这个提交会被广播给其他参选人，每个参选人都可以拿到其他参选人的提交。

在开启阶段，参选人提交一个开启状态，这个状态带着一个值，可以解开第一步提交的数据。同样，每个人都可以拿到其他人的开启值。

最后恢复阶段，每个参选人相当于都拿到了所有的提交和开启，如果参选人中存在不诚实的人，那么很有可能有些不提交，或者不提交开启的情况出现，在这种情况下，诚实的选民可以张贴（上面有提到）来重建密钥，这个想法很简单：即使某些选民是对手，选举也能成功结束。每个诚实的参选人解出来的都是同一个密钥，也就是随机种子。

这就是Cardano多方计算的出来的结果。

**VRF**

**Verifiable random function**  (VRF) ，同样是计算随机元的一个方法，其应用代表是Algorand和Dfinity 。VRF的描述其实很简单：

它是一种伪随机函数，可以在不提供输入值的情况下，验证结果的正确性。给定输入值x，秘钥SK的所有者可以计算函数值y = FSK（x）和证明pSK（x）。 使用证明和公钥\&lt;math\&gt; PK = g ^ {SK} \&lt;/ math\&gt;，每个人都可以检查值y = FSK（x）是否确实正确计算，整个过程并不需要用到所有者的私钥。

**其他**

业界还有一些选举算法，Vitalik曾经写过一篇文章，专门介绍了选举算法的不同优缺点。链接：[https](https://vitalik.ca/files/randomness.html)[://vitali](https://vitalik.ca/files/randomness.html)[k.ca/fi](https://vitalik.ca/files/randomness.html)[les](https://vitalik.ca/files/randomness.html)[/randomness.](https://vitalik.ca/files/randomness.html)[html](https://vitalik.ca/files/randomness.html)

其中如NXT的RNG算法，RANDAO算法等等，有兴趣的小伙伴可以看一下。

**伪随机数源**

随机数分为真随机数和伪随机数，真随机数是真是发生在现实中的随机事件产生的结果，比如扔硬币，扔骰子等等，伪随机数是用确定性的算法计算出来自[0,1]均匀分布的随机数序列，其实伪随机数并不是真正意义上的随机数。因为产生伪随机数是用确定的算法，那么只要是输入是一样的，那么输出肯定是一样的，也就意味着，输入源一旦被确定，那么输出是肯定会被知道的。

PoS出块人不能被提前被人预测出来，如果顺序能被知道，那么出块人提前勾结形成卡特尔组织，对系统是有一定危害的（在没有出现抵押金-抵扣的策略之前，这种攻击代价更小）。所以，PoS中的随机算法输入源显得非常非常重要。

在Tezos中，要求在下一个选举周期之前，该周期所有出块人必须披露一个Nonce，所有的Nonce会被收集到一个列表，通过Scrypt密钥衍生函数得出种子，该种子用于下个选择周期的随机种子输入源。

EOS中，在每个出块周期开始时，会根据持币人所投票数选出21个区块生产者。被选中的区块生产者的顺序会根据15个及以上的区块生产者的同意，制定出块顺序的安排。

在Cadona中，使用多方计算（multiparty computation (MPC) ）的方法来获取随机种子，简称扔硬币（Coin tossing）。简单来说就是这个方法模拟了现实中扔骰子的过程，来最终确定随机种子的生成，需要每个出块人都扔一次硬币得到一个随机数，然后互相分享扔硬币的结果，然后对所有结果进行种子生成，如果所有人都生成了一样的结果，那么说明大家互相使用了一样的结果。这就确认了种子生成。

Cadano和Tezos的随机种子生成很相似，只不过实现细节上有不一样。Cadano用多方计算保证了即使有人没有分享结果，也能通过部分结果恢复成整体。而Tezos要求每个出块人都批量Nonce，否则会被扣除抵押金（Slash）。相当于Cadano用了数学的方法解决了批量的问题，Tezos用了规则来限定，各有千秋了。


## 1.5. **打包交易**

轮到出块人出块的时候，出块人会从交易池里面挑选交易交易，并验证交易后，将其打包到区块中。

验证的过程就是对交易做一些基本的判断，比如判断交易合不合法，签名是否正确，交易账户是否有足够的余额，交易是否已经被花出，交易数据大小是否已经超出限制等，不同PoS公有链对于交易的判断不同，验证的条件也不同。

另外，从交易池子里面挑选交易，一般会有一个选择逻辑。该选择逻辑会参考两个因素，一个是交易手续费的大小，一个是发起交易的时间点。一般情况下，出块人的节点设置会优先选择手续费大的进行打包，同时系统也会设定一定参数，将交易时间点靠前的交易加入打包。当然，出块人还可以设置一些参数来调整打包程序，比如选择交易额大的，占内存空间小的等等，但是，不可以设置一些对地址交易的优先级，如果可以，那么该PoS机制的抗审查能力会被质疑。


## 1.6. **广播交易**

出块人将新区块生成后，会将区块数据广播到网络当中，为了让所有节点都把这些交易记录到自己的网络当中，这样全网维护的账本才能统一。网络中其他的节点会在不同的时间接收到广播的数据，新的节点会对打包数据进行进一步确认，并对其签名做认证，只有通过的验证数据对其进记录该节点本地所记录的账本当中。如果没有认证通过，那么节点将拒绝对其签名，同时，也不会将节点数据继续广播到其他节点中去。

广播通过点对点（Peer to Peer P2P）的网络进行传播。在点对点的网络当中，所有节点都是平等的，并且广播的传递并不会以物理地域为界限选择节点广播，而是会和网络中和自己连接的节点为传播，1传多，多传广，这样节点信息传播会以病毒扩散般的速度扩散开来。

虽然广播的传递不是以最近原则为基础，但是广播传播的速度会受到地域的影响，广播到达各个节点的时间并不一致，所以会出现一个数据同步问题。出块人通过节点A进行了广播，节点B在100ms后接受到了数据包，节点C则是在1000ms后接收到了数据包，这种延迟的时间间隔，会让整个网络有被攻击的机会出现，比如双花攻击、短程攻击等等，结果会影响交易的最终确定性，通常为了使得整个网络消灭延迟，通常交易的最终确定需要一定区块的时间确认。


## 1.7. **验证人确认**

PoS当中，矿工除了会出块外，还有充当验证人的角色。为了保证足够快的交易确认，每个区块的需要一定数量的验证人来进行签名确认，来保证其交易的合法性。

当前高度下，当出块人出块并广播后，被验证人接受到广播，紧接着对区块数据进行验证，在确认后用私钥签名，并继续广播出去。假设每个块需要10个签名，那么只有当网络中的所有的验证人都签名后，这个块才会被认为是最终的区块，并且填充到新的高度上来。

所以在出块人广播后，每个节点接收到广播，并维护这区块的一个中心状态，比如当系统只有1个签名的时候，节点并不能承认这个块是一个最终块，只有当该节点收到该区块新的签名更新，直到数量更新到10的时候，该节点才会把区块写到最高的高度上来的。

当然，为了保证签名的顺利性，整个区块的确认机制需要保证被提前选出来的验证人不会作恶，不在线或者有时间延迟等问题，可替代的解决方案有验证人备选，判断最小时间延迟，Slash作恶等。

所以，其实每个由节点所运行的PoS网络当中，节点运行的其实都是一个服务程序，这个程序会检测很多状态，以此来确认该区块的最终确定性。比如，检测是否有签名权利，检测是否可以替代签名，检测区块的最终签名状态等，不同的检测结果会触发接下来的很多操作，其目的都是为了保证整个区块链能顺利并且稳定的运行下去，不至于某个验证人不在线，整个网络崩溃的问题出现。

至此，PoS共识的7个步骤就说完了，整个过程从运行人的角度来说，从运行节点到区块验证确认；从区块高度角度说，就一个个新块打包生成的一个过程，每个区块链的共识基本都是这样，不断往复循环的一个过程。