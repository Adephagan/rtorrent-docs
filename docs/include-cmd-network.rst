.. _network-commands:

`network.*` commands
^^^^^^^^^^^^^^^^^^^^

.. glossary::

    network.bind_address
    network.bind_address.set

        **TODO**

    network.http.dns_cache_timeout
    network.http.dns_cache_timeout.set

        .. code-block:: ini

           network.http.dns_cache_timeout.set = ‹seconds› ≫ 0
           network.http.dns_cache_timeout ≫ ‹seconds›

        Controls the `DNS cache expiry <https://curl.haxx.se/libcurl/c/CURLOPT_DNS_CACHE_TIMEOUT.html>`_
        (in seconds) for HTTP requests. The default is 60 seconds.

        Set to zero to completely disable caching, or set to -1 to
        make the cached entries remain forever.


    network.http.current_open
    network.http.max_open
    network.http.max_open.set

        .. code-block:: ini

           network.http.current_open ≫ value ‹num›
           network.http.max_open ≫ value ‹max›
           network.http.max_open.set = ‹max› ≫ 0

        ``network.http.current_open`` returns the number of currently opened HTTTP connections,
        and ``network.http.max_open`` determines the upper limit for simultaneous HTTP connections.

        Be wary of setting this too high, as even if your connection can support
        that many requests, the target host may not be able to respond quickly enough,
        leading to timeouts.


    network.http.proxy_address
    network.http.proxy_address.set

        **TODO**

    network.http.cacert
    network.http.cacert.set
    network.http.capath
    network.http.capath.set

        **TODO**

    network.http.ssl_verify_host
    network.http.ssl_verify_host.set
    network.http.ssl_verify_peer
    network.http.ssl_verify_peer.set

        **TODO**

    network.listen.backlog
    network.listen.backlog.set
    network.listen.port

        **TODO**

    network.local_address
    network.local_address.set

        **TODO**

    network.max_open_files
    network.max_open_files.set

        **TODO**

    network.max_open_sockets
    network.max_open_sockets.set
    network.open_sockets

        **TODO**

    network.port_open
    network.port_open.set
    network.port_random
    network.port_random.set
    network.port_range
    network.port_range.set

        **TODO**

    network.proxy_address
    network.proxy_address.set

        **TODO**

    network.receive_buffer.size
    network.receive_buffer.size.set
    network.send_buffer.size
    network.send_buffer.size.set

        .. code-block:: ini

           network.receive_buffer.size ≫ value ‹size›
           network.receive_buffer.size.set = ‹size› ≫ 0
           network.send_buffer.size ≫ value ‹size›
           network.send_buffer.size.set = ‹size› ≫ 0

        Sets or gets the maximum socket receive / send buffer in bytes.

        On Linux, the default buffer size for receiving data is set by the
        ``/proc/sys/net/core/rmem_default`` file (``wmem_default`` for sending).
        The maximum allowed value is set by the ``/proc/sys/net/core/rmem_max`` file
        (``wmem_max`` for sending).

        See the `tuning guide <https://github.com/rakshasa/rtorrent/wiki/Performance-Tuning#networking-tweaks>`_
        for tweaking these values


    network.scgi.dont_route
    network.scgi.dont_route.set

        .. code-block:: ini

           network.scgi.dont_route ≫ bool (0 or 1)
           network.scgi.dont_route.set = ‹bool› ≫ 0

        Enable / disable routing on SCGI connections,
        directly calling `setsockopt <https://linux.die.net/man/3/setsockopt>`_
        to modify the ``SO_DONTROUTE`` flag.


    network.scgi.open_local
    network.scgi.open_port

        .. code-block:: ini

           network.scgi.open_local = string ‹path› ≫ 0
           network.scgi.open_port = string ‹domain_or_ip›:‹port› ≫ 0

        Open up a Unix domain socket or a TCP port for SCGI communication (i.e. the XMLRPC socket).
        Only use *one* of these!

        .. note::

            Using ``network.scgi.open_port`` means *any* user on the machine you run *rTorrent* on can
            execute *arbitrary* commands with the permission of the *rTorrent* runtime user.
            Most people don't realize that, now you do! Also, **never** use any other address than
            ``127.0.0.1`` with it.


    network.tos.set

        .. code-block:: ini

           network.tos.set = ‹flag› ≫ 0

        Set the `type of service <https://en.wikipedia.org/wiki/Type_of_service>`_
        flag to use in IP packets.

        The options as pulled from :term:`strings.ip_tos` are:

        .. hlist::
            :columns: 3

            * ``default``
            * ``lowdelay``
            * ``throughput``
            * ``reliability``
            * ``mincost``

        ``default`` uses the system default setting.
        A raw hexadecimal value can also be passed in for custom flags.


    network.xmlrpc.dialect.set

        .. code-block:: ini

           network.xmlrpc.dialect.set = ‹dialect [value 0…2]› ≫ 0

        Set the XMLRPC dialect to use, as defined by  ``xmlrpc-c``.
        The ``dialect`` parameter can have these values:

        * 0 – ``dialect_generic``
        * 1 – ``dialect_i8``
        * 2 – ``dialect_apache``

        ``dialect_i8`` is the default value, which means the XMLRPC API will use the
        `xmlrpc-c i8 extension type <http://xmlrpc-c.sourceforge.net/doc/libxmlrpc.html#extensiontype>`_
        for returning long integers.

        See `its documentation <http://xmlrpc-c.sourceforge.net/doc/libgeneral.html#dialect>`_
        for more information on how ``xmlrpc-c`` handles dialects.


    network.xmlrpc.size_limit
    network.xmlrpc.size_limit.set

        .. code-block:: ini

           network.xmlrpc.size_limit = ≫ value ‹bytes›
           network.xmlrpc.size_limit.set = ‹max-size› ≫ 0

        Set or return the maximum size of any XMLRPC requests in bytes.
        Human-readable forms such as ``2M`` are also allowed (for 2 MiB, i.e. 2097152 bytes).


    network.history.auto_scale
    network.history.auto_scale.set
    network.history.depth
    network.history.depth.set
    network.history.refresh
    network.history.sample

        Commands to add network traffic charts to the bottom of the collapsed
        download display.

        Add these lines to your configuration:

        .. code-block:: ini

            # rTorrent-PS 0.*+ only!

            # Show traffic of the last hour (112*32 = 3584 ≈ 3600)
            network.history.depth.set = 112

            method.insert = network.history.auto_scale.toggle, simple|private,\
                "branch=(network.history.auto_scale),\
                    ((network.history.auto_scale.set, 0)),\
                    ((network.history.auto_scale.set, 1))"
            method.insert = network.history.auto_scale.ui_toggle, simple|private,\
                "network.history.auto_scale.toggle= ; network.history.refresh="

            schedule2 = network_history_sampling, 1, 32, "network.history.sample="
            schedule2 = bind_auto_scale, 0, 0,\
                "ui.bind_key=download_list, =, network.history.auto_scale.ui_toggle="

        This will add the graph above the footer,
        you get the upper and lower bounds of traffic
        within your configured time window, and each bar of the graph
        represents an interval determined by the sampling schedule.

        Pressing ``=`` toggles between a graph display with base line 0,
        and a zoomed view that scales it to the current bounds.


`ip_tables.*` commands
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. glossary::

    ip_tables.add_address
    ip_tables.get
    ip_tables.insert_table
    ip_tables.size_data

        **TODO**


`ipv4_filter.*` commands
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. glossary::

    ipv4_filter.add_address
    ipv4_filter.dump
    ipv4_filter.get
    ipv4_filter.load
    ipv4_filter.size_data

        **TODO**

.. END cmd-network
