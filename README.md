thinkphp-annotation
=======
前言:
-------

thinkphp5.1

用注解的方式在控制器中实现：

- 数据验证
- 获取参数
- 属性对象注入



安装
------------

```bash
composer require cshaptx4869/thinkphp-annotation
```



## 配置

`tags.php `添加行为

```php
'action_begin' => [
     \Fairy\ControllerAnnotationScaner::class
]
```

添加 `system.php` 配置文件（可选）

```php
return [
    'annotation' => [
        'cache' => false,//是否开启注解读取缓存
        'writelist' => []//注解读取白名单
    ]
]
```

> PS：默认验证器注解验证不通过会终止程序运行并返回`json`格式的验证错误信息。



## 使用

`ArticleController` 控制器：

```php
<?php

namespace app\index\controller;

use app\index\validate\Article\SaveValidate;
use app\common\model\ArticleModel;
// 引入对应的注解
use Fairy\Annotation\Autowire;
use Fairy\Annotation\RequestParam;
use Fairy\Annotation\Validator;
use think\Request;

class ArticleController
{
    /**
     * 属性对象注入
     * class: 类名(必填) string类型
     * @Autowire(class=ArticleModel::class)
     */
    public $articleModel;
    
    /**
     * 数据验证
     * clsss: thinkphp定义的验证器类名(必填) string类型
     * scene: 验证场景名 string类型
     * batch：是否批量验证 bool类型
     * throw: 验证失败是否抛出异常 bool类型
     * @Validator(
     *     class=SaveValidate::class,
     *     scene="save",
     *     batch=false,
     *     throw=false
     * )
     *
     * 获取参数
     * fields: 定义要获取的字段名，可批量设置默认值 array类型
     * mapping: 转换前台传递的字段名为自定义的字段名 array类型
     * method: 获取参数的方法,支持get、post、put、delte string类型
     * @RequestParam(
     *     fields={"title","image_url","content","is_temporary","extra":"默认值"},
     *     mapping={"image_url":"img_url"},
     *     method="post"
     * )
     */
    public function save(Request $request)
    {
        //获取过滤过后的参数
        $postData = $request->requestParam;

        return MyToolkit::success($this->articleModel->store($postData));
    }
}
```



## IDE 注解插件支持

一些ide已经提供了对注释的支持，推荐安装，以便提供注解语法提示

- Eclipse via the [Symfony2 Plugin](http://symfony.dubture.com/)
- PHPStorm via the [PHP Annotations Plugin](http://plugins.jetbrains.com/plugin/7320) or the [Symfony2 Plugin](http://plugins.jetbrains.com/plugin/7219)

