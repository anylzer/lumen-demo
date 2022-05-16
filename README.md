# Lumen With Lavavel-s 

## Lumen 安装 

```bash
composer create-project --prefer-dist laravel/lumen lumen-swoole
```

[Lumen Install](https://lumen.laravel.com/docs/9.x)

## Laravel-s 安装 

### 安装 

```bash
composer require "hhxsv5/laravel-s:~3.7.0"
```

[Laravel-s document](https://github.com/hhxsv5/laravel-s/blob/master/README-CN.md)

### 注册 Service Provider

- `Lumen`: 修改文件`bootstrap/app.php`
    ```php
    $app->register(Hhxsv5\LaravelS\Illuminate\LaravelSServiceProvider::class);
    ```
### 发布 
    ```bash
    php artisan laravels publish
    # 配置文件：config/laravels.php
    # 二进制文件：bin/laravels bin/fswatch bin/inotify
    ```

### 运行 
> `在运行之前，请先仔细阅读：`[注意事项](https://github.com/hhxsv5/laravel-s/blob/master/README-CN.md#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)(这非常重要)。

- 操作命令：`php bin/laravels {start|stop|restart|reload|info|help}`。

| 命令 | 说明 |
| --------- | --------- |
| start | 启动LaravelS，展示已启动的进程列表 "*ps -ef&#124;grep laravels*" |
| stop | 停止LaravelS，并触发自定义进程的`onStop`方法 |
| restart | 重启LaravelS：先平滑`Stop`，然后再`Start`；在`Start`完成之前，服务是`不可用的` |
| reload | 平滑重启所有Task/Worker/Timer进程(这些进程内包含了你的业务代码)，并触发自定义进程的`onReload`方法，不会重启Master/Manger进程；修改`config/laravels.php`后，你`只有`调用`restart`来完成重启 |
| info | 显示组件的版本信息 |
| help | 显示帮助信息 |

- 启动选项，针对`start`和`restart`命令。

| 选项 | 说明 |
| --------- | --------- |
| -d&#124;--daemonize | 以守护进程的方式运行，此选项将覆盖`laravels.php`中`swoole.daemonize`设置 |
| -e&#124;--env | 指定运行的环境，如`--env=testing`将会优先使用配置文件`.env.testing`，这个特性要求`Laravel 5.2+` |
| -i&#124;--ignore | 忽略检查Master进程的PID文件 |
| -x&#124;--x-version | 记录当前工程的版本号(分支)，保存在`$_ENV`/`$_SERVER`中，访问方式：`$_ENV['X_VERSION']` `$_SERVER['X_VERSION']` `$request->server->get('X_VERSION')` |

- `运行时`文件：`start`时会自动执行`php artisan laravels config`并生成这些文件，开发者一般不需要关注它们，建议将它们加到`.gitignore`中。

| 文件 | 说明 |
| --------- | --------- |
| storage/laravels.conf | LaravelS的`运行时`配置文件 |
| storage/laravels.pid | Master进程的PID文件 |
| storage/laravels-timer-process.pid | 定时器Timer进程的PID文件 |
| storage/laravels-custom-processes.pid | 所有自定义进程的PID文件 |

## License

The Lumen framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
