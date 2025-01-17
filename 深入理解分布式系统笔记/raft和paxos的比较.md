Paxos 协议有一个很大的设计假设，它要求支持多个投票，也就是数据库里的多条日志之间是可以乱序提交的，可以并行处理的。但是 Raft 协议做了一个约束，数据库的多个投票多条日志一定要按照顺序执行，只有前一个日志被确认了才能再确认后一个日志。Raft 协议给出了分布式一致性协议的一个比较简单的实现，这种简化使得 Paxos 协议走进了千家万户。

但是有得必有失，Raft 把这个约束变得更简单了以后，导致了两个问题，第一个问题是并发能力变差了。以前支持并发的提交，现在只能支持一个结束以后再进入下一个，所以它的性能变差了。第二个是可用性的问题。如果采用 Paxos 协议，当一台机器新上线的时候很快就能提供服务，因为不需要等前面的数据确认就能提供服务，但是如果使用的是 Raft 协议，需要等前面的所有日志确认以后才能提供服务，所以说 Raft 协议存在可用性的风险。在某些场景尤其是异地部署和比较差的网络环境下是有风险的。

在所有分布式系统里分为两个阵营，一个是 Paxos 的阵营，包括 Google Spanner，OceanBase 1.0 及其之后的版本，Amazon DynamoDB 等。另一个是 Raft 阵营，包括腾讯的 TDSQL 以及一系列的开源数据库，这其中基于 MySQL 的系统基本上都是通过 Raft 协议来实现的。