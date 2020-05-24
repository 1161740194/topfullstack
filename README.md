# topfullstack
nodejs vue 全栈项目 视频网站

# 初始化项目
## 创建服务端项目
nest new server
## 转换为monorepo模式结构
nest generate app admin
## 运行这个项目
nest start -w admin

# 数据库
## 创建一个公用数据库模块
nest g lib db
@libs
### 在admin里面的app.module.ts使用公用模块
imports[ DbModule ]

## 链接数据库以及配置
### 安装
yarn add nestjs-typegoose @typegoose/typegoose mongoose @types/mongoose
### 在libs/db/src/db.module.ts配置链接
import { TypegooseModule } from 'nestjs-typegoose'
imports: [
    TypegooseModule.forRoot('mongodb://localhost/topfullstack', {
      useNewUrlParser: true,
      useCreateIndex: true,
      useUnifiedTopology: true,
      useFindAndModify: false
    })
  ],
### 在libs/db/src创建数据库模型model并新建一个user.model.ts,并添加字段
import { Prop } from '@typegoose/typegoose'
export class User {
    @Prop()
    username: string

    @Prop()
    password: string
}
### 在db.module.ts进行全局引用,并进行导入和导出
const models = TypegooseModule.forFeature([User])
@Global()
imports[{models}]
exports: [models]
### 在admin里添加users模块和控制器
nest g mo -p admin users
nest g co -p admin users

## 接口文档
### 增删该查接口使用curd
 yarn add nestjs-mongoose-crud
### 使用swagger注释说明
 yarn add @nestjs/swagger swagger-ui-express
### 在main.ts配置swagger查看文档
https://docs.nestjs.com/recipes/swagger
### 在user.model.ts定义时间属性
@ModelOptions({
    schemaOptions: {
        timestamps: true
    }
})

