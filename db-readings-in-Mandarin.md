# 認識資料庫的閱讀清單

不論目的是深入認識資料庫或建立新的資料庫系統，以下清單的研究都是經典且重要的。此清單由Reynold Xin於[db-readings](https://github.com/rxin/db-readings)編輯和維護。

## 目錄 
-----

1. [基礎知識和演算法](##基礎知識和演算法)
2. [關聯式資料庫（Relational-Databases）的要點](##關聯式資料庫（Relational-Databases）的要點)
3. [經典的系統設計](##經典的系統設計)
4. [列式資料庫（Columnar-Databases）](##列式資料庫（Columnar-Databases）)
5. [資料平行運算](##資料平行運算)
6. [共識與一致性](##共識與一致性)
7. [趨勢（雲端運算、倉庫規模運算、新硬體）](##趨勢（雲端運算、倉庫規模運算、新硬體）)
8. [其他](##其他)
9. [外部連結清單](##外部連結清單)

## 基礎知識和演算法
-----

* **〈「五分鐘法則」提出的十年之後，以及其他電腦存儲的經驗法則〉（1997）**
    這篇論文（以及十年前提出「五分鐘法則」的論文）說明了一種用於計算「是否應該將資料頁面快取存儲在記憶體中」的定量公式。很高興讀到Jim Gray提出的方法解決一系列相關問題，像是：頁面(page)大小多大較為合適。

    文章連結：[The Five-Minute Rule Ten Years Later, and Other Computer Storage Rules of Thumb ](https://github.com/rxin/db-readings/blob/master/papers/5_min_rule_sigmod.pdf)(1997)

* AlphaSort: A Cache-Sensitive Parallel External Sort (1995): 
    排序是資料庫最重要的演算法之一，因為它在連結、聚合和排序上的作用。在演算法101課程中，電腦科學學生被要求論述big O的複雜度並且要忽略常數因子(constant factor)。然而在實際上，L2快取的常數變化幅度可能高達二至三個數量級。這是一篇很好瞭解實行快速排序所使用的所有技巧的論文。
    
    文章連結：[AlphaSort: A Cache-Sensitive Parallel External Sort ](https://github.com/rxin/db-readings/blob/master/papers/alphasortsigmod.pdf)(1995)

* Patience is a Virtue: Revisiting Merge and Sort on Modern Processors (2014): 
    此文重新檢視排序(sorting)，另一方面也是對實作上用到排序演算法和對於演算法取捨相當好的討論。
    
    文章連結：[Patience is a Virtue: Revisiting Merge and Sort on Modern Processors ](https://github.com/rxin/db-readings/blob/master/papers/patsort-sigmod14.pdf)(2014)
    
## 關聯式資料庫（Relational-Databases）的要點

-----

* Architecture of a Database System (2007):
    Joe Hellerstein對關聯式資料庫系統的出色概述。本文向讀者介紹了關聯式資料庫系統的必要組成。
    
    文章連結：[Architecture of a Database System](https://github.com/rxin/db-readings/blob/master/papers/fntdb07-architecture.pdf)(2007)
    
* A Relational Model of Data for Large Shared Data Banks (1970): 
    Codd對資料獨立性的主張（自1970年起）是關聯式資料庫中的一個基本概念。儘管目前趨勢是NoSQL，但我認為這篇論文的想法在大規模平行處理資料系統正在變得越來越重要。
    
    文章連結：[A Relational Model of Data for Large Shared Data Banks](https://github.com/rxin/db-readings/blob/master/papers/codd.pdf) (1970)

* ARIES: A Transaction Recovery Method Supporting Fine-Granularity Locking and Partial Rollbacks Using Write-Ahead Logging (1992): 
    第一個真正有效的演算法：即使在出現故障的情況下，它也支援交易（transactions）的同步化處理而不丟失資料。本文因為在解釋高階演算法中融入了許多底層細節，因此十分不易閱讀，或許在閱讀此篇論文之前先讀資料庫的教科書理解ARIES（日誌復原）（log recovery）比較好。
    
    文章連結：[ARIES: A Transaction Recovery Method Supporting Fine-Granularity Locking and Partial Rollbacks Using Write-Ahead Logging](https://github.com/rxin/db-readings/blob/master/papers/aries.pdf) (1992)

* Efficient Locking for Concurrent Operations on B-Trees (1981) 和
  The R -tree: An Efficient and Robust Access Method for Points and Rectangles (1990): 
    B\*-Tree是資料庫中的核心資料結構（而不僅僅是關聯式）。它是經過優化的，並且具有較低的讀取放大因子幫助對磁碟資料進行隨機查詢。R-tree是B-tree的擴充，用來支援多維度資料查詢，例如地理資料。
    
    文章連結：[Efficient Locking for Concurrent Operations on B-Trees](https://github.com/rxin/db-readings/blob/master/papers/btree.pdf) (1981)
    文章連結：[The R-tree: An Efficient and Robust Access Method for Points and Rectangles](https://github.com/rxin/db-readings/blob/master/papers/rstar-tree.pdf) (1990)

* Improved Query Performance with Variant Indexes (1997): 
    分析式資料庫和線上交易處理（OLTP）資料庫需要不同的權衡取捨，這反映在選擇的索引（indexing）資料結構上。本文討論了許多適合用在分析式資料庫的索引資料結構。
    
    文章連結：[Improved Query Performance with Variant Indexes](https://github.com/rxin/db-readings/blob/master/papers/variant-index.pdf)(1997)
    
* On Optimistic Methods for Concurrency Control (1981): 
    支持同步化的方法有兩種：第一種是消極的方式，意即用鎖將共享的資料鎖住；第二種是本文介紹的一種稱為「積極同步化控制」的替代鎖定方法。積極的方法假設衝突很少發生，且執行交易(transactions)時不需要獲取鎖，在確認交易前資料庫系統會檢查是否有衝突，如果有衝突則中止/重新交易。
    
    文章連結：[On Optimistic Methods for Concurrency Control](https://github.com/rxin/db-readings/blob/master/papers/occ.pdf) (1981)

* Access Path Selection in a Relational Database Management System (1979): 
    查詢優化的基礎。SQL是宣告式的，意思是運用查詢語言指定需要的資料(what)，而不是取得的方式(how)。執行查詢通常有多種方式（多個查詢計畫），資料庫系統檢測多個計劃並盡可能選擇最佳方案，這個過程稱為查詢優化，傳統上進行查詢優化的方法是為不同的存取方法和查詢計畫建立成本模型。此論文解說了成本模型和用來選擇最佳計畫的動態規劃演算法。

    文章連結：[Access Path Selection in a Relational Database Management System](https://github.com/rxin/db-readings/blob/master/papers/systemr-optimizer.pdf) (1979)

* Eddies: Continuously Adaptive Query Processing (2000): 
    傳統的查詢優化（和使用的成本模型）是靜態的，傳統模式有兩個問題：第一、缺少資料統計下很難建立成本模型；第二、長時間運作查詢的情況下，查詢執行的環境可能有變動，而靜態的方法無法捕捉到變動。與流體力學相似，此論文提出了一組可動態優化查詢執行的技術。我認為渦流的想法還沒實踐在商業界，但此論文令人耳目一新，並且現今可能變得愈發重要。

    文章連結：[Eddies: Continuously Adaptive Query Processing](https://github.com/rxin/db-readings/blob/master/papers/eddies.pdf) (2000)

## 經典的系統設計

-----

* A History and Evaluation of System R (1981): 
    IBM的System R和柏克萊大學的Ingres，這兩個系統在當時展現了關聯式資料庫的可行性。本文描述了System R，令人印象深刻且驚駭的是2012年關聯式資料庫系統的內部和1981年的System R非常相似。
    
    文章連結：[A History and Evaluation of System R](https://github.com/rxin/db-readings/blob/master/papers/systemr.pdf)(1981)

* The Google File System (2003) 和 
  Bigtable: A Distributed Storage System for Structured Data (2006): 
    關於Google資料基礎建設的兩個核心部分：GFS是用於大型順序讀取（資料密集型應用程式）的附加寫入（append-only）分散式檔案系統；BigTable則是建立在GFS之上的高性能分散式資料存儲。一種理解方式是將GFS視為為了高傳輸量而進行的優化，而BigTable則說明了如何在GFS之上建立低延遲的資料存儲。現在其中一些技術可能已被Google內部新的專有技術取代，但這些想法仍然保留著。
    
    文章連結：[The Google File System](https://github.com/rxin/db-readings/blob/master/papers/gfs.pdf)(2003)
    文章連結：[Bigtable: A Distributed Storage System for Structured Data](https://github.com/rxin/db-readings/blob/master/papers/bigtable.pdf) (2006)

* Chord: A Scalable Peer-to-peer Lookup Service for Internet Applications (2001) 和
  Dynamo: Amazon’s Highly Available Key-value Store (2007):   
    Chord出現在分散式雜湊表是熱門研究的時代，它的用途只有一個並且作用極佳：如何在一個完全分散設定中（peer-to-peer）用一致性Hash的方式查找鍵（key）的位置。Dynamo論文介紹了如何使用Chord建立分散式鍵值存儲。請注意，某些設計上決定採用Dynamo而非Chord（例如：finger table O(logN) vs O(N)）是因為在Dynamo的情況下，Amazon對資料中心的節點有更多的控制權，然而Chord假定了在廣域網中使用對等節點（peer-to-peer）。
    
    文章連結：[Chord: A Scalable Peer-to-peer Lookup Service for Internet Applications](https://github.com/rxin/db-readings/blob/master/papers/chord.pdf) (2001)
    文章連結：[Dynamo: Amazon’s Highly Available Key-value Store](https://github.com/rxin/db-readings/blob/master/papers/dynamo.pdf) (2007)
    
## 列式資料庫（Columnar-Databases）

-----

列式存儲和列導向查詢引擎對於分析工作負載十分重要，例如：OLAP。自問世以來（1999年MonetDB論文）已二十年，現今幾乎每個商業倉儲資料庫都有列式引擎。

* C-Store: A Column-oriented DBMS (2005) 和
  The Vertica Analytic Database: C-Store 7 Years Later (2012): 
    C-Store是一個由新英格蘭人完成具影響力的學術系統。Vertica是C-Store的商業展現。
    
    文章連結：[C-Store: A Column-oriented DBMS](https://github.com/rxin/db-readings/blob/master/papers/cstore.pdf) (2005)
    文章連結：[The Vertica Analytic Database: C-Store 7 Years Later](https://github.com/rxin/db-readings/blob/master/papers/vertica-7-years.pdf) (2012)

* Column-Stores vs. Row-Stores: How Different Are They Really? (2012): 
    論述了列式存儲和列式查詢引擎此兩者的重要性。
    
    文章連結：[Column-Stores vs. Row-Stores: How Different Are They Really?](https://github.com/rxin/db-readings/blob/master/papers/column-vs-row.pdf) (2012)

* Dremel: Interactive Analysis of Web-Scale Datasets (2010): 
    這篇文章在當時Google發佈時，非常令人吃驚。Dremel是Google用來進行即時查詢的大型平行分析式資料庫，該系統在數千個節點上執行，可以在幾秒鐘內處理數TB（terabytes）的資料，它將列式存儲應用在復雜且巢狀的資料結構。
    此論文討論了很多關於巢狀資料結構所支援(the nested data structure support)，並且對執行「查詢」的細節有一些著墨。值得注意的是，許多開源項目宣稱它們在構建Dremel系統，Dremel系統是透過大規模平行方式和列式存儲實踐了低延遲，然而因為世界上很少有公司能夠負擔成千上萬個一次性/即時查詢節點，因此這個模型在Google外部不一定有意義。

   文章連結：[Dremel: Interactive Analysis of Web-Scale Datasets](https://github.com/rxin/db-readings/blob/master/papers/dremel.pdf)(2010)

## 資料平行運算

-----

* MapReduce: Simplified Data Processing on Large Clusters (2004): 
    MapReduce既是程式設計模型（源於函式語言程式設計的舊概念），也是Google上用於分散式資料密集型運算的系統。該程式設計是如此簡潔卻令人驚艷於可以滿足廣泛的程式設計需求；這個與模型結合的系統具有容錯能力和可擴展性，大概可以這麼說：現在學術界在研究的問題其中一半都是受到MapReduce極大影響。

    文章連結：[MapReduce: Simplified Data Processing on Large Clusters](https://github.com/rxin/db-readings/blob/master/papers/mapreduce.pdf)(2004)

* Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing (2012): 
    這是柏克萊大學Spark的叢集計算計劃研究論文。Spark展現了一種稱為RDD的分散式記憶體抽象化，它是分散在整個叢集記憶體中的記錄的不可變集合，可以使用MapReduce方式計算來轉換RDD；對於有強時間局部性的工作負載（例如：查詢處理和反覆的機器學習），RDD抽象化可以提高數個數量級的效率。Spark是一個說明「為什麼將MapReduce程式設計模型與其執行引擎分開是重要的」的例子。
    
    文章連結：[Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing](https://github.com/rxin/db-readings/blob/master/papers/spark.pdf) (2012)

* Shark: SQL and Rich Analytics at Scale (2013): 
    本論文描述了Shark系統，Shark系統是個建立在Spark之上的SQL引擎。更重要的是，本論文討論了為什麼先前在Hadoop / MapReduce查詢引擎上的SQL速度慢。
    
    文章連結：[Shark: SQL and Rich Analytics at Scale](https://github.com/rxin/db-readings/blob/master/papers/shark.pdf) (2013)
    
## 共識與一致性

-----

* Paxos Made Simple (2001): 
    Paxos是一種容錯的分散式共識協議，它構成了各種分散式系統的基礎。此論文論述的想法很簡單，但是眾所周知地很難理解（也可能是因為Paxos論文的撰寫方式）。

    文章連結：[Paxos Made Simple](https://github.com/rxin/db-readings/blob/master/papers/paxos.pdf)(2001)

* The Raft Consensus Algorithm (2014) : 
    Raft是一種共識演算法，意圖是用來替代Paxos。透過邏輯分離，Raft的目標是希望比Paxos更容易理解，並且已被正式證明是安全的、具有一些新特色。Raft提供了一種在計算系統叢集中分散狀態機的通用方法，可確保叢集中的每個節點都同意相同的一系列狀態轉換。
    
    文章連結：[The Raft Consensus Algorithm](https://github.com/rxin/db-readings/blob/master/papers/raft.pdf)(2014)

* CAP Twelve Years Later: How the "Rules" Have Changed (2012): 
    Eric Brewer提出的CAP定理主張任何網絡共享數據系統只能具有三個理想屬性中的其中兩個：一致性、可靠性和分區容忍性，許多NoSQL存儲引用CAP定理來證明其犧牲一致性的決定是合理的。這是Eric Brewer對CAP定理的回顧文章，解釋說「『三個中的兩個』這樣的表達總是令人誤解，因為它傾向於過分簡化特性之間的張力（tensions among properties）。」
    
    文章連結：[CAP Twelve Years Later: How the "Rules" Have Changed](https://github.com/rxin/db-readings/blob/master/papers/cap.pdf) (2012)
    

## 趨勢（雲端運算、倉庫規模運算、新的硬體）

-----

* A View of Cloud Computing (2010): 
    這是有關雲端運算的文章。這篇文章從技術角度討論了雲端運算的經濟和阻礙（關於資源的靈活度，而不是面對消費者的「雲端」）。本文提出的阻礙將影響運行在雲端系統的設計決策。
    
    文章連結：[A View of Cloud Computing](https://github.com/rxin/db-readings/blob/master/papers/cloud-computing.pdf) (2010)

* The Datacenter as a Computer: An Introduction to the Design of Warehouse-Scale Machines:
    Google的LuizAndréBarroso和UrsHölzle解釋了用於倉庫規模計算的資料中心硬體和軟體的基礎知識。有一個[配合這篇文章的影片](https://dl.acm.org/doi/10.1145/2024723.2019527?bnc=1)，這個影片敘述了在大型平行系統中減少長尾延遲的重要性；在此另一個重要的想法是資源的分解，由於高網絡頻寬像是GFS/HDFS之類的技術已經對磁碟進行了分解，但是由於低延遲網絡的需求因此目前尚未看到DRAM有相同的趨勢。
    
    文章連結：[The Datacenter as a Computer: An Introduction to the Design of Warehouse-Scale Machines](https://github.com/rxin/db-readings/blob/master/papers/data-center-computer.pdf)
    
## 其他

-----

* Reflections on Trusting Trust (1984): 
    Ken Thompson在1984年的圖靈獎獲獎感言中，描述了黑箱後門問題（black box backdoor issues），並且指出信任並非理所當然的。
    
    文章連結：[Reflections on Trusting Trust](https://github.com/rxin/db-readings/blob/master/papers/trusting-trust.pdf)(1984)

* What Goes Around Comes Around: 
    Michael Stonebraker和Joseph M. Hellerstein摘要了三十五年來資料模型的諸多提案，這篇文章將這些主張以九個時代分類，分別討論了每個時代的提案，並表示其中內含的建立資料模型的基本想法寥寥、大同小異，並且大多數想法已經存在很長時間了。後來的提案難以避免地與先前某些提案非常相似。
    
    文章連結：[What Goes Around Comes Around](https://github.com/rxin/db-readings/blob/master/papers/goes-around.pdf)
    
## 外部連結閱讀清單

-----

許多學校都有各自給研究生的資料庫閱讀清單。

* [布朗大學 CSCI 2270資料庫管理進階主題](https://www.francosolleza.com/CS227/)
* [史丹佛大學博士資格考試](http://infolab.stanford.edu/db_pages/infoqual.html)
* [麻省理工學院：資料庫系統6.830　2010年](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-830-database-systems-fall-2010/readings/)
* [威斯康辛大學資料庫資格考試閱讀清單](https://www.cs.wisc.edu/wp-content/uploads/sites/871/2018/12/Database-systems-qual_Summer-2014.pdf)（2014）
* [卡內基美隆大學 15-721資料庫系統閱讀列表（2016年春季）](https://15721.courses.cs.cmu.edu/spring2016/schedule.html)
