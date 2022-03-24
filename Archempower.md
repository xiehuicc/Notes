192.168.21.100  工行测试环境

账号：suer

密码：123456@schx

192.168.21.199 nginx  

### 一、出入

本地测试环境：http://localhost:9100/entry/web?id_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50X2lkIjoiNjExMzRhYzJkOTI1ZWU1YWRiODVlZjE5IiwiaWRlbnRpdHlJZCI6IjYxMTM0YWMyZDkyNWVlNWFkYjg1ZWZjMyIsInRlbmFudElkIjoiNjExMzRhYzJkOTI1ZWU1YWRiODVlZjE4IiwidGVuYW50TmFtZSI6IuW3pemTtuWQjuWLpCIsInRlbmFudElkZW50aXR5IjoiemhxIiwicGVvcGxlSWQiOiI2MTEzNGFjMmQ5MjVlZTVhZGI4NWVmMTkiLCJuYW1lIjoi5bel6ZO25ZCO5YukIiwidGVsIjoiNjExMzRhYzJkOTI1ZWU1YWRiODVlZjE5IiwiY2xpZW50VHlwZSI6MSwiZXhwaXJlZF90aW1lIjoiMjAyMS0wOC0xNyAxODoxNDowNyIsImlhdCI6MTYyOTE2NjQ0N30.Ji5DHrmwrlUF6VO2rRLGIUUDRAiguXPMbuW_UtuDec4



https://preview-gyds.ctron.com.cn/operate/?id_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50X2lkIjoiNjA3ZTdlNjFjMmY3NGEwMDE4MjY0ZTkzIiwiaWRlbnRpdHlJZCI6IjYwN2U3ZTYyYzJmNzRhMDAxODI2NGYyNCIsInRlbmFudElkIjoiNjA3ZTdlNjFjMmY3NGEwMDE4MjY0ZTkyIiwidGVuYW50TmFtZSI6IuW3pemTtuWkp-WOpiIsInRlbmFudElkZW50aXR5IjoiYWJjIiwicGVvcGxlSWQiOiI2MDdlN2U2MWMyZjc0YTAwMTgyNjRlOTMiLCJuYW1lIjoi5bel6ZO25aSn5Y6mIiwidGVsIjoiNjA3NzYxMjc0MDAxODI2NDkzIiwiY2xpZW50VHlwZSI6MSwiZXhwaXJlZF90aW1lIjoiMjAyMS0wOC0yMCAyMTo0MTo0MyIsImlhdCI6MTYyOTQzODEwM30.f-dSFglKeCsBNmykaVK-OOanG3c-MDPBtbvzcbd9ACM&refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50X2lkIjoiNjA3ZTdlNjFjMmY3NGEwMDE4MjY0ZTkzIiwiZXhwaXJlZF90aW1lIjoiMjAyMS0wOC0yNyAxMzo0MTo0MyIsImlhdCI6MTYyOTQzODEwM30.a-pMmbhoYB-qvHf4ILdaave0It3KHd8FViiGYQ7l75o&people_id=607e7e61c2f74a0018264e93&identity_id=607e7e62c2f74a0018264f24&tenant_id=607e7e61c2f74a0018264e92

第三方

  entryobject/eoAuthorityController  md5



基础平台中添加的字段，如果出入中想要查询出来，在出入中必须经过一层转换才可以



**面包屑问题**

****





### 二、基础平台

**jenkins**：https://tools-allinone.ctron.com.cn/jenkins/login

账号/密码： projects/123456



**项目启动配置：**

```js
client/web/static/config.js
	const baseUrl = 'http://localhost:7010/iobp/' // 本地 'http://127.0.0.1:7010/iobp/'
	const FileUploadBase_entry = 'https://preview-gyds.ctron.com.cn/file/'
	const FileThumbBase_entry = 'https://preview-gyds.ctron.com.cn/file/'
	const FileUploadBase = 'https://preview-gyds.ctron.com.cn/file/'
	const FileThumbBase = 'https://preview-gyds.ctron.com.cn/file/'
	const domain = "preview-gyds.ctron.com.cn"
api和service下的config中的env.js 中的namespace 改为nacos配置
例如：service/aechdata/config/env.js 	
	export const config = {
   	 configServer: 'nacos://nacos:nacos@192.168.22.10:8848/nacos',
    	 namespace: 'f09a86cf-1688-4a78-b6e2-e455f128bcb1'
	};
```



**bug1**：角色列表中的绑定角色，无法加载人员列表  

报错：lodash.map错误

解决：替换掉lodash.map方法，直接使用js原生方法，filter



****

**请求拦截**

在`client/web/src/utils/axios`中对发送的请求前会进行拦截

白名单过滤，不传token（whiteList）

添加白名单，则在发送请求时对该url不进行拦截

```js
axios.get('/app/login/verificationCode',{params: {uniformId: this.ruleForm.uniformId}})

this.$store.dispatch('verificationCode',{uniformId: this.ruleForm.uniformId})
其中通过dispatch发送请求，传入的参数会自带一层params，所以上面两行参数格式一致
```



**service配置租户id白名单**

租户id白名单：

nacos中的service.archdata

更改过nacos配置后，测试环境  -->可以docker重启服务而不用重新构建代码



**线上环境报错，可以通过docker进行查看日志**

```
docker logs [镜像名称] 

```



**内网环境访问amap报错bug**

>内网环境访问不到互联网资源，导致请求amap地图服务失败
>
>本地复现：
>
>window中修改 hosts文件 	/c/Windows/System32/drivers/etc 下的hosts
>
>改变amap的映射地址

例如：

```js
127.0.0.1       webapi.amap.com
127.0.0.1       webapi.amap.com
```

https://www.jianshu.com/p/cff21d7ff016 关于hosts文件

对人员进行按部门查询

先根据部门名称查询organizations表，并将_id构造一个查询条件去查询peoples表中的orginazation中的 _id 字段



**不明原因代码**

```js
  if (peopleList.results) {
          orgList.filter(ele => {
             if (filter.params.identities.$elemMatch.$or[3].organization.organization.$regex === ele.organization) {
               filter["$or"].push({
                 parentOrganizationId:orgList.organizationId
            }) 
           }
        })
            peopleList = await Promise.all([
            mongodbCrud.query(peopleCollentionName, filter),
           ])
        }
```



****

### 三、运营平台

本地测试环境：http://localhost:7000/operate/?id_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50X2lkIjoiNjA3ZTdlNjFjMmY3NGEwMDE4MjY0ZTkzIiwiaWRlbnRpdHlJZCI6IjYwN2U3ZTYyYzJmNzRhMDAxODI2NGYyNCIsInRlbmFudElkIjoiNjA3ZTdlNjFjMmY3NGEwMDE4MjY0ZTkyIiwidGVuYW50TmFtZSI6IuW3pemTtuWkp-WOpiIsInRlbmFudElkZW50aXR5IjoiYWJjIiwicGVvcGxlSWQiOiI2MDdlN2U2MWMyZjc0YTAwMTgyNjRlOTMiLCJuYW1lIjoi5bel6ZO25aSn5Y6mIiwidGVsIjoiNjA3NzYxMjc0MDAxODI2NDkzIiwiY2xpZW50VHlwZSI6MSwiZXhwaXJlZF90aW1lIjoiMjAyMS0wOC0xMiAyMTozNzozNyIsImlhdCI6MTYyODc0NjY1N30.1MmIK2tNKiyc-AdfYTlvK3i8WodomW0QDidyZ-5fyo0&refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50X2lkIjoiNjA3ZTdlNjFjMmY3NGEwMDE4MjY0ZTkzIiwiZXhwaXJlZF90aW1lIjoiMjAyMS0wOC0xOSAxMzozNzozNyIsImlhdCI6MTYyODc0NjY1N30.PbhPAnm4J7ozvvftprI1RLArpAYCAj1FGCL2PgEZnl4&account_id=607e7e62c2f74a0018264f24&people_id=607e7e61c2f74a0018264e93&identity_id=607e7e62c2f74a0018264f24&tenant_id=607e7e61c2f74a0018264e92



**解除加密：**

api：config/config.default.js 文件中 将中间件配置中的*encipher*注释

web ：isEncrypt：false



### 1.同步人员

1.首先再物业下建立一个租户，将租户id写入nacos，再进行人员同步

2.人员中系统管理员没有部门id，暂放在集团组织中，将集团id写入nacos

3.角色问题

