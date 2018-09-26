# lager error_logger_redirect issue

## The issue

`error_logger_redirect` not working on OTP-21. But it works on OTP 20:


## Code

lager_issue_sup.erl:

```erlang
init([]) ->
    lager:info("this is output of lager:info"),
    error_logger:info_msg("this is output of error_logger:info_msg"),
    {ok, { {one_for_all, 0, 1}, []} }.
```

## OTP-21

With OTP-21, the message `this is output of error_logger:info_msg` is printed twice:

```shell

$ rebar3 shell

=INFO REPORT==== 26-Sep-2018::20:17:36.447950 ===
this is output of error_logger:info_msg

...

20:17:36.446 [info] this is output of lager:info
20:17:36.448 [info] this is output of error_logger:info_msg
20:17:36.448 [info] Application lager_issue started on node nonode@nohost

1>
1> application:get_all_env(lager).
[{error_logger_hwm,50},
 {crash_log_count,5},
 {crash_log_date,"$D0"},
 {crash_log,"log/crash.log"},
 {error_logger_redirect,true},
 {colored,false},
 {async_threshold,20},
 {async_threshold_window,5},
 {crash_log_msg_size,65536},
 {colors,[{debug,"\e[0;38m"},
          {info,"\e[1;37m"},
          {notice,"\e[1;36m"},
          {warning,"\e[1;33m"},
          {error,"\e[1;31m"},
          {critical,"\e[1;35m"},
          {alert,"\e[1;44m"},
          {emergency,"\e[1;41m"}]},
 {crash_log_size,10485760},
 {crash_log_rotator,lager_rotator_default}]
2>
```


## OTP 20

```shell
➜  ./rebar3 shell
===> Verifying dependencies...
===> Compiling lager_issue
Erlang/OTP 20 [erts-9.3.3.1] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:1] [hipe] [kernel-poll:false] [dtrace]

Eshell V9.3.3.1  (abort with ^G)
1>
21:08:15.125 [info] Application lager started on node nonode@nohost
21:08:15.129 [info] this is output of lager:info
21:08:15.129 [info] this is output of error_logger:info_msg
...

1>
```