---
layout:     post
title:      Use "dd" to Benchmark Your Disk or CPU
date:       2016-05-21
summary:    Repost from romanrm.net about use "dd" to benchmark disk on linux
categories: linux
---

### Storage benchmark (hard disk or SSD)

Ways in which you can invoke `dd` to test the write speed:

{% highlight bash %}
dd bs=1M count=256 if=/dev/zero of=test
{% endhighlight %}

The default behaviour of `dd` is to not "sync" (i.e. not ask the OS to completely write the data to disk before `dd` exiting). The above command will just commit your 128 MB of data into a RAM buffer (write cache) – this will be really fast and it will show you the hugely inflated benchmark result right away. However, the server in the background is still busy, continuing to write out data from the RAM cache to disk.

{% highlight bash %}
dd bs=1M count=256 if=/dev/zero of=test; sync
{% endhighlight %}

Absolutely identical to the previous case, as anyone who understands how `*nix` shell works should surely know that adding a `; sync` does not affect the operation of previous command in any way, because it is executed independently, after the first command completes. So your (inflated) MB/sec value is already printed on screen while that sync is only preparing to be executed.

{% highlight bash %}
dd bs=1M count=256 if=/dev/zero of=test conv=fdatasync
{% endhighlight %}

This tells `dd` to require a complete "sync" once, right before it exits. So it commits the whole 256 MB of data, then tells the operating system: "OK, now ensure this is completely on disk", only then measures the total time it took to do all that and calculates the benchmark result. Recommended to use. If your server or VPS is really fast and the above test completes in a second or less, try increasing the `count= number` to `1024` or so, to get a more accurate averaged result.

{% highlight bash %}
dd bs=1M count=256 if=/dev/zero of=test oflag=dsync
{% endhighlight %}

Here dd will ask for completely synchronous output to disk, i.e. ensure that its write requests don't even return until the submitted data is on disk. In the above example, this will mean sync'ing once per megabyte, or 256 times in total. It will be the slowest mode, as the write cache is basically unused at all in this case.


### CPU benchmark

Perhaps not many people use this, but `dd` in conjunction with any stream-processing CPU-intensive program can also be used as a simple CPU benchmark! It may be not very accurate, but the huge advantage is that it doesn't require installing any additional software whatsoever, and typically you can run this "out of the box" on any GNU/Linux system. The usage is as follows:

{% highlight bash %}
dd if=/dev/zero bs=1M count=1024 | md5sum
{% endhighlight %}

In this case I used the `md5sum` program, which calculates the MD5 hash of data that is fed to it. In effect, `dd` here fetches 1 GB of zeroes from the Linux kernel, feeds that to md5sum, and then prints how fast in MB/sec that was processed. So for example on a modern 3.6 GHz AMD Phenom II CPU:

{% highlight bash %}
$ dd if=/dev/zero bs=1M count=1024 | md5sum
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 2.28735 s, 469 MB/s
cd573cfaace07e7949bc0c46028904ff  -
{% endhighlight %}

And on a VPS with a 2.1 GHz Opteron:

{% highlight bash %}
# dd if=/dev/zero bs=1M count=1024 | md5sum
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 5.14561 s, 209 MB/s
cd573cfaace07e7949bc0c46028904ff  -
{% endhighlight %}

Generally on any modern 2.0+ GHz CPU you should be looking at a result of 200 MB/sec or more in this test. If you see very low results, like 20-40-50-100 MB/sec, it's a sure sign that whatever system you're running this on is either overloaded CPU-wise, or is hard-limiting your CPU allowance to only some portion of a full CPU core.

Basicly this post just copy and paste from [here](https://romanrm.net/dd-benchmark).