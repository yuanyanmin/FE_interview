# Nest.js 装饰器

## 内置装饰器

* 核心类
```
@Catch：异常过滤器装饰器
@Controller：控制器装饰器
@UseFilters：异常过滤器装饰器
@Inject：显示依赖声明装饰器
@Injectable：被依赖装饰器
@Optional：可选装饰器
@SetMetadata：设置源数据装饰器
@UseGuards：守卫装饰器
@UseInterceptors：拦截器装饰器
@UsePipes：管道装饰器
```
* Http类
```
@Header：响应头装饰器
@HttpCode：Http状态码装饰器
@Redirect：重定向装饰器
@Render：渲染装饰器
@RequestMapping：请求映射装饰器
@Request、@Response、@Next、@Ip、@Session、@Headers：路由参数装饰器
@Query、@Body、@Param、@HostParam：管道路由装饰器
@UploadFile：文件上传装饰器
@Sse：SSE装饰器
```

* 模块类

```
@Global: 全局装饰器
@Module：模块装饰器
```
