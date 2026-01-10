# 特征工程

> Using intuition to design **new features**, 基于业务理解 + 基于常用套路 + 基于EDA
> 
> 我个人喜欢基于类似[OOD设计](../03_system/01_ood/README.md)的ER图进行思考

- numerical feature
- categorical feature
- sparse feature
- dense feature
- time feature
- sequence feature
- graph feature


## 特征生成

- 类型角度: 比如数值类和类别类
- domain角度: 比如推荐常用的用户，物品，context, 交叉特征


## 特征选择

- correlation filtering

## Feature store

> 数据经过feature engineering pipeline之后进入feature store，feature store负责特征的**存储与serving**
- Ingestion: Batch (ETL, Spark) and Stream (Flink, Kafka)
- Store: Offline (parquet, BigQuery) and Online (Redis)
- Service: offline and online access


![](../.github/assets/02ml-feature_store.png)


## 问答

- high cardinality
- feature backfill: backfill offline first → experiment → promote only if useful.
  - feature lifestyle: draft → backfill → validate → promote → deprecate
  - [How Pinterest Accelerates ML Feature Iterations via Effective Backfill](https://medium.com/pinterest-engineering/how-pinterest-accelerates-ml-feature-iterations-via-effective-backfill-d67ea125519c)


## 参考

**精读**

- [特征工程到底是什么？ - 砍手豪的回答 - 知乎](https://www.zhihu.com/question/29316149/answer/2346832545)
- [Feature Engineering](https://www.slideshare.net/slideshow/feature-engineering-72376750/72376750)

**扩展**

- [刀功：谈推荐系统特征工程中的几个高级技巧 - 石塔西的文章 - 知乎](https://zhuanlan.zhihu.com/p/448680238)
- [Feature Store 101](https://medium.com/data-for-ai/feature-store-101-b964373891c4)
- [有哪些精彩的特征工程案例？ - 冯国添的回答 - 知乎](https://www.zhihu.com/question/400064722/answer/1911094226)
- [A Closer Look at Our Machine Learning Feature Store](https://www.binance.com/en/blog/all/3411614684128221181)
- [Lessons from Building a Feature Store on Flink](https://medium.com/airwallex-engineering/lessons-from-building-a-feature-store-on-flink-4604d8fb9c80)

**代码**

- [https://github.com/senkin13/kaggle](https://github.com/senkin13/kaggle)
- [https://www.kaggle.com/code/fabiendaniel/elo-world](https://www.kaggle.com/code/fabiendaniel/elo-world)
