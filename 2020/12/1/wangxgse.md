# 提交说明
- 编号：NO.26@20210717
- 文章：[Immutability Changes Everything](http://cidrdb.org/cidr2015/Papers/CIDR15_Paper16.pdf)

# 读书笔记

## INTRODUCTION
> Increasing Storage, Distribution, & Ambiguity

存储越来越廉价，而服务面临分布式环境越来越复杂，进而带来更多的难以处理的问题(Can you take action with incomplete knowledge? Can you wait for enough knowledge)
>  We can now afford to keep immutable copies of lots of data, and one payoff is reduced coordination challenges.

我们可以通过数据的不可变性来降低分布式环境下的协调同步的复杂度

> Immutability is a key architectural concept at many layers of the stack.

不可变性对于各个层面的技术实现产生了影响，从硬件（如SSD HDD）到存储系统（如HDFS,GFS,LSF），再到应用层的大数据，甚至我们的应用，我们的app。

## Accountants Don’t Use Erasers
> May kinds of computing are “Append-Only”. Observations are recorded forever (or for a long time). Derived results are calculated on demand (or periodically pre-calculated).

##  Data on the Outside vs. Data on the Inside
> There are deep differences in the representation, meaning, and usage of inside data versus outside data. Increasingly, we are keeping data as outside (immutable) data.

## Referencing Immutable Data
> When we have snapshots or some form of isolation, database data becomes semantically immutable for the duration of the calculation.

## Immutability Is in the Eye of the Beholder
- > Massively parallel computations are based on immutable inputs and functional calculations. MapReduce [3] and Dryad [9] both take immutable files as input. The work is cut into pieces, each with immutable input. This functional calculation (using immutable inputs) is idempotent. It is OK to fail and restart.
- > Immutability is the Backbone of “Big Data”

## Versions Are Immutable, Too!
- > Versions are immutable and should have immutable names.
- > LSM presents a facade of change atop immutable files.

## Keeping the Stone Tablets Safe
- > GFS (Google File System) and HDFS (Hadoop Distributed File System) provide immutable files. Immutable blocks (chunks) are replicated across data nodes. Immutable files are a sequence of blocks (chunks) each of which is identified with a GUID. The contents of a file are immutable and labeled with a GUID. The file-id GUID always refers to exactly one file and its contents.
- > By separating the namespace from block placement control, there are a number of advantages. The consistent hashing ring can take writes and reads even when the ring is under flux.

## Hardware Changes towards Unchanging
- > 【对硬件领域产生了影响】The trend to leverage immutability in new designs is so pervasive we see it in a number of hardware areas.
- > 【SSD的损耗均衡】Wear leveling is a form of copy-on-write and treats each version of the block as an immutable version.
- > 【shingled disk】
    - > In shingled disks, there is a large band of data that is written as layered write tracks forming a shingle pattern, partially overwriting the preceding tracks. The data in the middle of the band cannot be overwritten without trashing the remaining part of the band.
    - > To overcome this, the hardware disk controllers implement log- structured file systems within the disk controller [14]. The operating system is unaware of the use of shingles. What’s written to the disk (i.e. the band of data written with shingles) remains unchanging until it is discarded. The user of the disk (e.g. the operating system) perceives the ability to update in place.
## Immutability May Have Some Dark Sides
- Denormalization: Nimble but Fat
- Write Amplification vs. Read Perspiration

## Conclusion
> Designs are driving towards immutability. We need immutability to coordinate at ever increasing distances. We can afford immutability given room to store data for a long time. Versioning gives us a changing view of things while the underlying data is expressed with new contents bound to a unique identifier.