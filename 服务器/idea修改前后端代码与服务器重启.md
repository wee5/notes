# idea修改前后端代码与服务器重启

## （restart）debug和正常运行模式下重启服务器

| 正常模式      | Update | Replay | Reserve |
| ------------- | ------ | ------ | ------- |
| Jsp，静态资源 | o      | o      | o       |
| Java代码      | x      | o      | o       |

| Debug         | Update | Replay | Reserve |
| ------------- | ------ | ------ | ------- |
| Jsp，静态资源 | o      | o      | o       |
| Java代码      | o      | o      | o       |

总结：html，css，js都是直接生效的，但前端页面需要重新获取。正常模式下update不会更新java代码，而debug模式下会更新java代码

