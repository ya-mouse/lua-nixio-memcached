Name
====

lua-nixio-memcached - Lua memcached client driver on the Nixio API

Originaly based on [lua-resty-memcached](https://github.com/openresty/memc-nginx-module#keep-alive-connections-to-memcached-servers) client.

Table of Contents
=================

* [Name](#name)
* [Status](#status)
* [Description](#description)
* [Synopsis](#synopsis)
* [Methods](#methods)
    * [connect](#connect)
    * [set](#set)
    * [set_timeout](#set_timeout)
    * [set_keepalive](#set_keepalive)
    * [get_reused_times](#get_reused_times)
    * [close](#close)
    * [add](#add)
    * [replace](#replace)
    * [append](#append)
    * [prepend](#prepend)
    * [get](#get)
    * [gets](#gets)
    * [cas](#cas)
    * [touch](#touch)
    * [flush_all](#flush_all)
    * [delete](#delete)
    * [incr](#incr)
    * [decr](#decr)
    * [stats](#stats)
    * [version](#version)
    * [quit](#quit)
    * [verbosity](#verbosity)
* [TODO](#todo)
* [Author](#author)
* [Copyright and License](#copyright-and-license)
* [See Also](#see-also)

Status
======

This library is considered production ready.

Description
===========

This Lua library is a memcached client driver.

Synopsis
========

```lua
        local memcached = require "nixio.memcached"
        local memc, err = memcached:connect("127.0.0.1", 11211)
        if not memc then
            print("failed to connect: ", err)
            return
        end

        memc:set_timeout(1000) -- 1 sec

        local ok, err = memc:flush_all()
        if not ok then
            print("failed to flush all: ", err)
            return
        end

        local ok, err = memc:set("dog", 32)
        if not ok then
            print("failed to set dog: ", err)
            return
        end

        local res, flags, err = memc:get("dog")
        if err then
            print("failed to get dog: ", err)
            return
        end

        if not res then
            print("dog not found")
            return
        end

        print("dog: ", res)

        local ok, err = memc:close()
        if not ok then
            print("failed to close: ", err)
            return
        end
```

[Back to TOC](#table-of-contents)

Methods
=======

The `key` argument provided in the following methods will be automatically escaped according to the URI escaping rules before sending to the memcached server.

[Back to TOC](#table-of-contents)

connect
-------
`syntax: memc, err = memcached:connect(host, port)`

Attempts to connect to the remote host and port that the memcached server is listening to or a local unix domain socket file listened by the memcached server.

Before actually resolving the host name and connecting to the remote backend, this method will always look up the connection pool for matched idle connections created by previous calls of this method.

[Back to TOC](#table-of-contents)

set
---
`syntax: ok, err = memc:set(key, value, exptime, flags)`

Inserts an entry into memcached unconditionally. If the key already exists, overrides it.

The `value` argument could also be a Lua table holding multiple Lua
strings that are supposed to be concatenated as a whole
(without any delimiters). For example,

```lua
    memc:set("dog", {"a ", {"kind of"}, " animal"})
```

is functionally equivalent to

```lua
    memc:set("dog", "a kind of animal")
```

The `exptime` parameter is optional, defaults to `0`.

The `flags` parameter is optional, defaults to `0`.

[Back to TOC](#table-of-contents)

set_timeout
----------
`syntax: ok, err = memc:set_timeout(time)`

Sets the timeout (in ms) protection for subsequent operations, including the `connect` method.

Returns 1 when successful and nil plus a string describing the error otherwise.

[Back to TOC](#table-of-contents)

set_keepalive
------------
`syntax: ok, err = memc:set_keepalive(max_idle_timeout)`

You can specify the max idle timeout (in ms) when the connection is in the pool.

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

close
-----
`syntax: ok, err = memc:close()`

Closes the current memcached connection and returns the status.

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.


[Back to TOC](#table-of-contents)

add
---
`syntax: ok, err = memc:add(key, value, exptime, flags)`

Inserts an entry into memcached if and only if the key does not exist.

The `value` argument could also be a Lua table holding multiple Lua
strings that are supposed to be concatenated as a whole
(without any delimiters). For example,

```lua
    memc:add("dog", {"a ", {"kind of"}, " animal"})
```

is functionally equivalent to

```lua
    memc:add("dog", "a kind of animal")
```

The `exptime` parameter is optional, defaults to `0`.

The `flags` parameter is optional, defaults to `0`.

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

replace
-------
`syntax: ok, err = memc:replace(key, value, exptime, flags)`

Inserts an entry into memcached if and only if the key does exist.

The `value` argument could also be a Lua table holding multiple Lua
strings that are supposed to be concatenated as a whole
(without any delimiters). For example,

```lua
    memc:replace("dog", {"a ", {"kind of"}, " animal"})
```

is functionally equivalent to

```lua
    memc:replace("dog", "a kind of animal")
```

The `exptime` parameter is optional, defaults to `0`.

The `flags` parameter is optional, defaults to `0`.

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

append
------
`syntax: ok, err = memc:append(key, value, exptime, flags)`

Appends the value to an entry with the same key that already exists in memcached.

The `value` argument could also be a Lua table holding multiple Lua
strings that are supposed to be concatenated as a whole
(without any delimiters). For example,

```lua
    memc:append("dog", {"a ", {"kind of"}, " animal"})
```

is functionally equivalent to

```lua
    memc:append("dog", "a kind of animal")
```

The `exptime` parameter is optional, defaults to `0`.

The `flags` parameter is optional, defaults to `0`.

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

prepend
-------
`syntax: ok, err = memc:prepend(key, value, exptime, flags)`

Prepends the value to an entry with the same key that already exists in memcached.

The `value` argument could also be a Lua table holding multiple Lua
strings that are supposed to be concatenated as a whole
(without any delimiters). For example,

```lua
    memc:prepend("dog", {"a ", {"kind of"}, " animal"})
```

is functionally equivalent to

```lua
    memc:prepend("dog", "a kind of animal")
```

The `exptime` parameter is optional, defaults to `0`.

The `flags` parameter is optional, defaults to `0`.

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

get
---
`syntax: value, flags, err = memc:get(key)`
`syntax: results, err = memc:get(keys)`

Get a single entry or multiple entries in the memcached server via a single key or a talbe of keys.

Let us first discuss the case When the key is a single string.

The key's value and associated flags value will be returned if the entry is found and no error happens.

In case of errors, `nil` values will be turned for `value` and `flags` and a 3rd (string) value will also be returned for describing the error.

If the entry is not found, then three `nil` values will be returned.

Then let us discuss the case when the a Lua table of multiple keys are provided.

In this case, a Lua table holding the key-result pairs will be always returned in case of success. Each value corresponding each key in the table is also a table holding two values, the key's value and the key's flags. If a key does not exist, then there is no responding entries in the `results` table.

In case of errors, `nil` will be returned, and the second return value will be a string describing the error.

[Back to TOC](#table-of-contents)

gets
----
`syntax: value, flags, cas_unique, err = memc:gets(key)`

`syntax: results, err = memc:gets(keys)`

Just like the `get` method, but will also return the CAS unique value associated with the entry in addition to the key's value and flags.

This method is usually used together with the `cas` method.

[Back to TOC](#table-of-contents)

cas
---
`syntax: ok, err = memc:cas(key, value, cas_unique, exptime?, flags?)`

Just like the `set` method but does a check and set operation, which means "store this data but
  only if no one else has updated since I last fetched it."

The `cas_unique` argument can be obtained from the `gets` method.

[Back to TOC](#table-of-contents)

touch
---
`syntax: ok, err = memc:touch(key, exptime)`

Update the expiration time of an existing key.

Returns `1` for success or `nil` with a string describing the error otherwise.

This method was first introduced in the `v0.11` release.

[Back to TOC](#table-of-contents)

flush_all
---------
`syntax: ok, err = memc:flush_all(time?)`

Flushes (or invalidates) all the existing entries in the memcached server immediately (by default) or after the expiration
specified by the `time` argument (in seconds).

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

delete
------
`syntax: ok, err = memc:delete(key)`

Deletes the key from memcached immediately.

The key to be deleted must already exist in memcached.

In case of success, returns `1`. In case of errors, returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

incr
----
`syntax: new_value, err = memc:incr(key, delta)`

Increments the value of the specified key by the integer value specified in the `delta` argument.

Returns the new value after incrementation in success, and `nil` with a string describing the error in case of failures.

[Back to TOC](#table-of-contents)

decr
----
`syntax: new_value, err = memc:decr(key, value)`

Decrements the value of the specified key by the integer value specified in the `delta` argument.

Returns the new value after decrementation in success, and `nil` with a string describing the error in case of failures.

[Back to TOC](#table-of-contents)

stats
-----
`syntax: lines, err = memc:stats(args?)`

Returns memcached server statistics information with an optional `args` argument.

In case of success, this method returns a lua table holding all of the lines of the output; in case of failures, it returns `nil` with a string describing the error.

If the `args` argument is omitted, general server statistics is returned. Possible `args` argument values are `items`, `sizes`, `slabs`, among others.

[Back to TOC](#table-of-contents)

version
-------
`syntax: version, err = memc:version(args?)`

Returns the server version number, like `1.2.8`.

In case of error, it returns `nil` with a string describing the error.

[Back to TOC](#table-of-contents)

quit
----
`syntax: ok, err = memc:quit()`

Tells the server to close the current memcached connection.

Returns `1` in case of success and `nil` other wise. In case of failures, another string value will also be returned to describe the error.

Generally you can just directly call the `close` method to achieve the same effect.

[Back to TOC](#table-of-contents)

verbosity
---------
`syntax: ok, err = memc:verbosity(level)`

Sets the verbosity level used by the memcached server. The `level` argument should be given integers only.

Returns `1` in case of success and `nil` other wise. In case of failures, another string value will also be returned to describe the error.

[Back to TOC](#table-of-contents)

TODO
====

* implement the memcached pipelining API.
* implement the UDP part of the memcached ascii protocol.

[Back to TOC](#table-of-contents)

Author
======

Yichun "agentzh" Zhang (章亦春) <agentzh@gmail.com>, CloudFlare Inc.
Adopted for NIXIO by Anton D. Kachalov <mouse@yandex-team.ru>, Yandex LLC.

[Back to TOC](#table-of-contents)

Copyright and License
=====================

This module is licensed under the BSD license.

Copyright (C) 2012-2016, by Yichun "agentzh" Zhang (章亦春) <agentzh@gmail.com>, CloudFlare Inc.
Copyright (C) 2016, by Anton D. Kachalov <mouse@yandex-team.ru>, Yandex LLC.

All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

[Back to TOC](#table-of-contents)

See Also
========
* the memcached wired protocol specification: http://code.sixapart.com/svn/memcached/trunk/server/doc/protocol.txt
* system, networking and I/O library for lua: http://luci.subsignal.org
* resty memcached client: https://github.com/openresty/lua-resty-memcached

[Back to TOC](#table-of-contents)

