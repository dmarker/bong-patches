[98]: https://reviews.freebsd.org/D49405
[99]: https://reviews.freebsd.org/D49158

[10]: https://github.com/dmarker/bong-kmods
[11]: https://github.com/dmarker/bong-utils


[20]: https://reviews.freebsd.org/D44615
[22]: https://reviews.freebsd.org/D50241
[23]: https://reviews.freebsd.org/D50244
[24]: https://reviews.freebsd.org/D50245

[30]: https://github.com/freebsd/freebsd-src/commit/86a6393a7d6766875a9e03daa0273a2e55faacdd
[31]: https://github.com/freebsd/freebsd-src/commit/46f38a6dedb1b474f04b7c2b072825fda5d7bd5a
[32]: https://github.com/freebsd/freebsd-src/commit/72d01e62b082de39ecf1ff3ced67dcf7259e5084
[33]: https://github.com/freebsd/freebsd-src/commit/685e60e860d61f6e1bcf981f5c30647e0c025702

# bong-patches

These are the required patches to FreeBSD to get [bong-kmods][10] and [bong-utils][11]
to work. They require you rebuild both `kernel` and `world` (for stable-14) but only
the kernel for `stable-15` and `current`. This is here to make
it easy for me to track and for others to try [bong-kmods][10] / [bong-utils][11].

I am working on getting the barest of minimum set into FreeBSD-16, the current
development branch. While I also run them on FreeBSD-14 and FreeBSD-15 (stable),
I do not have any plans to get any of these patches backported to FreeBSD-14. As
development moves to FreeBSD-16 I will at some point cease maintaining stable-14.

| Patch | Review       | Commit        | Description |
| :---- | :----------- | :------------ | :---------- |
|  0001 | [D44615][20] | [86a6393][30] | ng_bridge: allow to automatically assign numbers to new hooks |
|  0002 | N/A          | [46f38a6][31] | netgraph: Exit the net epoch to handle control messages |
|  0003 | [D50241][22] | [72d01e6][32] | Teach ngctl to attach and run itself in a jail. |
|  0004 | not ready    |               | Teach netgraph to parse IPv6 addresses |
|  0005 | not ready    |               | Always call if_ioctl for virtual interfaces. |


You can tell if a patch is merged by it having a commit.

`not ready` means I don't have the node that requires the patch yet. The patches themselves
are ready.

The second was not written by me, it was a bug I caused/exposed with my first
patch. The bug was mentioned and discussed in the same review [D44615][20] as it
broke automated tests.

There is one more you patch you need for FreeBSD-14 if you build your world without
jail support (EINCONCEIVABLE!): [685e60e][33].
Sigh, that makes 100% of my commits to date required a follow on fix for something
I feel like I should have caught myself.

The examples in [bong-utils][11] assume you have [86a6393][30] and [46f38a6][31].
The `jeiface` utility in [bong-utils][11] assumes you have [72d01e6][32]. Which you
should anyway for FreeBSD-15 or current.

Both `ng_siit` and `ng_nat64` will require the last two patches. There is no
way to use them without these.

My plan, if everything in table is merged, is to make 2 ports for [bong-kmods][10]
and [bong-utils][11].

I won't put [0004.patch](current/0004.patch) or [0005.patch](current/0005.patch)
up for review until the nodes that use them are ready. [0004.patch](current/0004.patch)
should not be controversial. [0005.patch](current/0005.patch) uses the same flag that
ng_iface(4) uses and I will not be surprised if I'm requested to use a new flag instead
(which is fine).

I don't think these patches are controversial. Whereas the nodes and utilities
in [bong-kmods][10] / [bong-utils][11] already appear so. I put up a review for
[ng_wormhole(4)][23] and [ngportal][24] but have abandoned them in preference
of [bong-kmods][10] / [bong-utils][11].

# 14-stable

Patches are unimaginatively named `0001.patch`, `0002.patch`, etc. for the order they
should be applied to your source tree.

I only update this when I update the systems I have still using FreeBSD-14 (all
on 14/stable). So the patches may need help to apply cleanly.


# 15-stable / current

The patches get the same numbers as 14/stable so if its missing here its merged.

