#Sencha Ext JS 国际化和本地化

> 对Ext JS的国际化和本地化做了些研究,有了些体会和总结,在此备忘.
> 首先将官方博客关于国际化和本地化的内容进行翻译,因为这篇文章很好的告诉我们该如何用Ext JS实现国际化和本地化,然后是根据自身项目的特点总结出一套自己的方案

##官方博客翻译

> 原文地址:[https://www.sencha.com/blog/internationalization-localization-with-sencha-ext-js/](https://www.sencha.com/blog/internationalization-localization-with-sencha-ext-js/)

Ext JS 提供"开箱即用"的国际化功能,并且很容易扩展.Ext JS 打包了超过40种本地化语言包,从印尼语到马其顿语,都很容易实现.

在本文中,我们将提供一些特殊情况和需求的解决方案,在这之前我们先看看所谓的国际化是个什么功能.

主要重点是应用程序要提供给用户选择语言的设置.这就要求应用程序是"可翻译"的,这样的功能常被称作"i18n"(internationalization单词取首尾,中间18个字母)或者"国际化"."可翻译"的意思不仅仅是将一个语言转换为另一个语言,还要考虑到与之关联的其他技术要求.

Sencha 为大多数情况下提供了良好的国际化和本地化解决方案,考虑到企业环境下的一些特殊需求,它也是可以扩展的.

###怎样本地化Ext JS应用程序

你可以从Sencha推荐的[官方文档](http://docs.sencha.com/extjs/6.0.2-classic/guides/core_concepts/localization.html)找到详细方法本地化你的应用程序.也有一些其他的推荐方法在社区成员Saki发的[博客](http://extjs.eu/localization-of-ext-applications/)里.接下来,我们将要提到:

	* 怎样直接在应用程序里写语言文件(通过singleton或override),这样它们就可以直接集成到产品构建中
	* 怎样生成,通过Sencha Cmd在相同应用程序中配置不同语言进行产品构建
	* 怎样每次改变语言后重新加载整个应用程序
	* 怎样启动应用程序之前轻松加载额外资源
	* 怎样在应用程序的不同部分使用不同的语言

![图片1][img1]

###需求来源

企业开发者和普通开发者是不一样的,因为他们有特殊的需求.当企业开发者选一个框架用时,他们要仔细小心的评估他们的用户需求.(看"[Sencha Day 2015](http://roadshow.senchacon.com/en/switzerland/zurich)" Jnesis技术主管 Vincent Munier的介绍)

具有一定规模的公司普遍得出同样的结论:跨开发框架的多语言管理是个麻烦事.适用于大型组织的多语言被关注,因为他们的商业进程在产品开发和企业发展的不同阶段需要不同语言.在某些情况下，语言要求定期更换,翻译的工作需要在应用外进行,大多是由第三方软件管理,并通过一个或多个Web服务加载到应用程序里.

这种方法的优点是:
* 改变语言不需要开发者重新构建应用-改变后立即生效.
* 翻译的工作不必由那个写代码的人来做.

可以这么认为,委托翻译的人由于不知道谁写的代码或者如何在JavaScript文件里编码而造成一些问题.可是,完全可以想到使用翻译层翻译内容,一个可以由非程序员使用，并且将数据加载到应用程序里.

如上所述,官方Ext JS方案在这里查看-[localization guide](http://docs.sencha.com/extjs/6.0.2-classic/guides/core_concepts/localization.html)

我们将描述我们的方法,它原生的实现了一个翻译层.项目一开始就使用良好的方法构建国际化到应用程序里是很重要的,可以避免痛苦的重构阶段.

###优雅的方法

首先,我们要深刻理解Sencha localization guide和Saki博客里的那篇文章:应用程序主屏构建之前加载语言数据.

这个方案适用于Ext JS 5之前的所有版本: 在应用程序的"index.html"文件里加入依赖到特殊的资源.

![图片2][img2]

为了实现这个,我们先要用一个"单例"的类来存储翻译变量,像这样:

```
Ext.define('Jnesis.Labels', {
    singleton: true,
    button: 'My english button',
    title: 'My english title'
});
```
这样,应用程序的任何地方都能访问译文了,怎样访问呢?就像打字一样简单:Jnesis.Labels.button 或 Jnesis.Labels.title
这样做比"重写"类的方法优点在于本地化的值在一个文件里且容易使用,因为像上面看到的那样,每个元素是一个变量.

一旦做到这一点,我们选择其他语言后只需要"重写"覆盖这个语言文件.有两个步骤:
1. 从web服务加载数据,服务器响应回的JSON看起来像这样:

```
{
    "button": "Mon Bouton",
    "title": "Mon titre"
}
```

我们需要解析数据并用解析到的值覆盖我们默认的本地化单例类:

```
Ext.define ('Jnesis.Application', {
    launch:  function () {
        Ext.Ajax.request({
            url: 'get-localization',
            params:{locale:'fr'},
            callback: function (options, success, response) {
                var data = Ext.decode(response.responseText, true);
                Ext.override(Jnesis.Labels, data);
                Ext.create('Jnesis.view.main.Main');
            }
        });
    }
});
```

2. 加载重写的Ext JS类:

```
Ext.define('Jnesis.locale.fr.Labels', {
    override: 'Jnesis.Labels',
    button: 'Mon Bouton',
    title: 'Mon titre'
});
```

然后用`Ext.Loader.loadscript` 方法处理它,当它完成后加载我们的主视图:

```
Ext.define ('Jnesis.Application', {
    launch: function () {
        var lang = 'fr',
            url = 'resources/locale/'+lang+'/Labels.js';
        Ext.Loader.loadScript({
            url: url,
            onLoad: function (options) {
                Ext.create('Jnesis.view.main.Main');
            }
        });
    }
});
```

然后，我们希望通过在component基类里定义一个新的属性(例如:"localized"),使用我们擅长的原生继承机制获取到翻译类属性.这个属性可以是由键,值组成的简单JavaScript对象(记住上面的Jnesis.Labels.button 和 Jnesis.Labels.title).

用定义键的字符串,就可以获取到对象的值:

```
Ext.define('Jnesis.view.main.Main', {
    extend: 'Ext.panel.Panel',
    localized: {
        title: 'Jnesis.Labels.title'
    },
    buttons: [{
        localized: {
            text: 'Jnesis.Labels.button'
        },
        handler: 'onClickButton'
    }]
});
```

一旦做到这一点,我们只需要重写"initComponent"的基类方法并将键的字符串转换为对象的值,那么我们就可以在任意Ext JS组件里使用了:

```
Ext.define('overrides.localized.Component', {
    override: 'Ext.Component',
    initComponent: function() {
        var me = this,
            localized = me.localized,
            value;
        if (Ext.isObject(localized)) {
            for (var prop in localized) {
                value = localized[prop];
                if (value) {
                    me[prop] = eval(value);
                }
            }
        }
        me.callParent(arguments);
    }
});
```

此解决方案以透明的方式在任何层级的组件声明(container 配置或 item 配置)正常运行.

##我的解决方案

> 综合官方方案、Saki博客和Sencha论坛网友提供的方法,再根据自身项目的修改,形成自己的解决方案.

###项目介绍

前端全部采用Ext Js开发的单页面Desktop应用程序,也就是使用了Ext Js里的Desktop组件加以修改.使用Sencha Cmd进行构建.

后端采用Java Spring MVC

修改需求:现需要将中文版加入英文版功能,并且可以随浏览器语言来识别显示中文版还是英文版,并且可以由用户自行修改语言,并记住语言.

需求分析:其实就是要做国际化和本地化,但后端和前端都有需要做国际化地方,所以统一由server端管理
	
	获取语言:当打开应用时浏览器通过js查询本地cookie并获取,
	设置语言:当用户点击指定语言,发送请求到server修改cookies里的语言并响应回由浏览器接收显示
	server端国际化由spring mvc完成
	前端国际化分为两部分
	A. 为项目内容的国际化,如产品,客户,用户登录等.
	B. 为Ext JS自身组件的国际化,例如必填字段提示文字,grid列头选择标题字等.

###实现A的方案

要想实现A,参考官方的方案即可,且可以做到无刷新更换语言.因为官方的方案是只写了一个类,另一种语言需要从后端请求JSON覆盖JS类的方法来实现,不够统一,我将其修改为以下方法:

定义一个单例的翻译类Locale,加入了多语言属性:

```
Ext.define('VSMS.Locale', {
	singleton: true,
	lang: 'zh_CN',
	username: {
		zh_CN: '用户名',
		en: 'Username'		
	},
	inputusername: {
		zh_CN: '输入用户名',
		en: 'Input username'		
	},
	login: {
		zh_CN: '登录',
		en: 'Login'
	},
	forgotpassword: {
		zh_CN: '忘记密码',
		en: 'Forgot password'
	},
	logout: {
		zh_CN: '注销',
		en: 'Logout'
	},
	Dywexit: {
		zh_CN: '确定要退出吗?',
		en: 'Exit?'
	},
	password: {
		zh_CN: '密码',
		en: 'Password'
	},
	inputpassword: {
		zh_CN: '输入密码',
		en: 'Input password'
	},
	checkcode: {
		zh_CN: '验证码',
		en: 'Check code'
	},
	changeone: {
		zh_CN: '换一张',
		en: 'Change'
	},
	autologin: {
		zh_CN: '自动登录',
		en: 'Auto login'
	}
})
```

Application初始化时初始化VSMS.Locale.lang:

```
Ext.define('VSMS.Application', {
	extend: 'Ext.app.Application',

	name: 'VSMS',

	stores: [
		// TODO: add global / shared stores here
	],
	launch: function() {
		//获取cookie 本地化语言
		var lang = Ext.util.Cookies.get('lang');
		if(!lang){
			lang = 'zh_CN';
		}
		VSMS.Locale.lang = lang;
	}
});
```

在Sencha Cmd项目里的overrides新建目录localized,并重写Component的initComponent方法:

![图片3][img3]

```
Ext.define('overrides.localized.Component', {
    override: 'Ext.Component',
    initComponent: function() {
        var me = this,
            localized = me.localized,
            value;
        if (Ext.isObject(localized)) {
            for (var prop in localized) {
                value = localized[prop];
                if (value) {
                    me[prop] = eval(value)[VSMS.Locale.lang];//根据不同语言来获取不同属性的值
                }
            }
        }
        me.callParent(arguments);
    }
});
```

在view上使用localized属性包住需要翻译的属性,完整的view太长,下面显示了一个片段:

```
items: [{
	xtype: 'textfield',
	name: 'username',
	localized: {
		fieldLabel: 'VSMS.Locale.username', //用户名
		emptyText: 'VSMS.Locale.inputusername' //输入用户名
	},
	fieldCls: 'logininput',
	beforeBodyEl: '<i id=\'iconuser\' class=\'fa fa-user loginicont loginiconu loginicon\'></i>',
	enableKeyEvents: true
		}]
```

那么在viewController里如何使用翻译属性呢?参照下面的写法:

```
VSMS.Locale.changeone[VSMS.Locale.lang]
```

到现在,所有A部分的国际化完成了.

###实现B的方案

我们知道,在用Sencha Cmd构建不同语言应用程序时需要在项目根目录的app.json指定:

```
    "requires": [
        "ext-locale"
    ]
```

```
    "locales": [
        "zh_CN"
    ],
```

*_注意: locales不要写成locale了,我因为写错了导致找了很久_*

可是在构建时locales只能构建一个语言.如果能构建多个语言,并通过一个"index.html"进行选择加载就能实现B的国际化了.思路如下图:

![图片1][img1]

下面是实现方法

修改Sencha Cmd项目根目录下app.json的以下属性:

```
    "requires": [
        "ext-locale"
    ],

    "locales": [
        "zh_CN",
        "en"
    ],

    "builds": {
        "crisp": {
            "theme": "ext-theme-crisp"
        }
    },

    "bootstrap": {
        "base": "${app.dir}",
        "manifest": "${build.id}.json",
        "microloader": "bootstrap.js",
        "css": "bootstrap.css"
    },

    "output": {
        "base": "${workspace.build.dir}/${build.environment}/${app.name}/${build.id}",
        "page": "../index.html",
        "manifest": "../${build.id}.json",
        "deltas": {
            "enable": false
        },
        "cache": {
            "enable": false
        },
        "js": {
            "optimize": {
                "callParent": true,
                "cssPrefix": true,
                "defines": true
            }
        }
    },
```

修改build.xml文件如下:

```
<?xml version="1.0" encoding="utf-8"?><project name="${app.name}" default=".help">
    <!--
    The build-impl.xml file imported here contains the guts of the build process. It is
    a great idea to read that file to understand how the process works, but it is best to
    limit your changes to this file.
    -->
    <import file="${basedir}/.sencha/app/build-impl.xml"/>

    <!--
    The following targets can be provided to inject logic before and/or after key steps
    of the build process:

        The "init-local" target is used to initialize properties that may be personalized
        for the local machine.

            <target name="-before-init-local"/>
            <target name="-after-init-local"/>

        The "clean" target is used to clean build output from the build.dir.

            <target name="-before-clean"/>
            <target name="-after-clean"/>

        The general "init" target is used to initialize all other properties, including
        those provided by Sencha Cmd.

            <target name="-before-init"/>
            <target name="-after-init"/>
        
        The "page" target performs the call to Sencha Cmd to build the 'all-classes.js' file.

            <target name="-before-page"/>
            <target name="-after-page"/>

        The "build" target performs the call to Sencha Cmd to build the application.

            <target name="-before-build"/>
            <target name="-after-build"/>
    -->

    <macrodef name="x-run-build">
        <attribute name="target"/>
        <attribute name="theme"/>
        <attribute name="locale" default="en"/>
        <attribute name="buildName" default="@{theme}"/>
        <attribute name="buildId" default="@{theme}-@{locale}"/>
        <sequential>
            <if>
                <equals arg1="@{locale}" arg2=""/>
                <then>
                    <ant dir="${basedir}" inheritall="false" inheritrefs="true" target="@{target}">
                        <property name="compiler.ref.id" value="${compiler.ref.id}-@{theme}"/>
                        <property name="app.theme" value="ext-theme-@{theme}"/>
                        <property name="app.build.dir.suffix" value="ext-theme-@{theme}"/>
                        <property name="cmd.dir" value="${cmd.dir}"/>
                        <property name="build.environment" value="${build.environment}"/>
                        <property name="build.compression.yui" value="${build.compression.yui}"/>
                        <property name="build.id" value="@{buildId}"/>
                        <property name="build.name" value="@{buildName}"/>
                    </ant>
                </then>
                <else>
                    <ant dir="${basedir}" inheritall="false" inheritrefs="true" target="@{target}">
                        <property name="compiler.ref.id" value="${compiler.ref.id}-@{theme}-@{locale}"/>
                        <property name="app.theme" value="ext-theme-@{theme}"/>
                        <property name="app.locale" value="@{locale}"/>
                        <property name="app.build.dir.suffix" value="ext-theme-@{theme}-@{locale}"/>
                        <property name="cmd.dir" value="${cmd.dir}"/>
                        <property name="build.environment" value="${build.environment}"/>
                        <property name="build.compression.yui" value="${build.compression.yui}"/>
                        <property name="build.id" value="@{buildId}"/>
                        <property name="build.name" value="@{buildName}"/>
                    </ant>
                </else>
            </if>
        </sequential>
    </macrodef>

    <target name="build-all" depends="build-crisp,build-crisp-touch"/>

    <!--<target name="clean" depends="init,clean-all"/>-->
    <!--<target name="build" depends="init,build-all"/>-->

    <!-- Build -->

    <target name="build-crisp" depends="init">
        <echo>Build ${app.name} - Crisp Theme</echo>
        <x-run-build theme="crisp" target="app-build-impl.build"/>
    </target>

    <target name="build-crisp-touch" depends="init">
        <echo>Build ${app.name} - Crisp Touch Theme</echo>
        <x-run-build theme="crisp-touch" target="app-build-impl.build"/>
    </target>

    <!-- Watch -->

    <target name="watch-crisp" depends="init">
        <echo>Watch ${app.name} - Crisp Theme</echo>
        <x-run-build theme="crisp" target="watch"/>
    </target>

    <target name="watch-crisp-touch" depends="init">
        <echo>Watch ${app.name} - Crisp Touch Theme</echo>
        <x-run-build theme="crisp-touch" target="watch"/>
    </target>

    <!-- Refresh -->

    <target name="refresh-crisp" depends="init">
        <echo>Refresh ${app.name} - Crisp Theme</echo>
        <x-run-build theme="crisp" target="app-build-impl.refresh"/>
    </target>

    <target name="refresh-crisp-touch" depends="init">
        <echo>Refresh ${app.name} - Crisp Theme</echo>
        <x-run-build theme="crisp-touch" target="app-build-impl.refresh"/>
    </target>

    <!-- Clean -->
    <target name="clean-all" depends="clean-crisp,clean-crisp-touch"/>

    <target name="clean-crisp" depends="init">
        <echo>Clean ${app.name} - Crisp Theme</echo>
        <x-run-build theme="crisp" target="app-build-impl.clean"/>
    </target>

    <target name="clean-crisp-touch" depends="init">
        <echo>Clean ${app.name} - Crisp Theme</echo>
        <x-run-build theme="crisp-touch" target="app-build-impl.clean"/>
    </target>

    <target name="sass-full" depends="resources,sass"/>

    <target name="sass-all" depends="init">
        <for param="theme" list="crisp,crisp-touch">
            <sequential>
                <x-run-build target="sass-full" theme="@{theme}"/>
            </sequential>
        </for>
    </target>
</project>
```

修改index.html文件如下:

```
<!DOCTYPE HTML><html><head>
    <meta charset="UTF-8">
    <title>Application Name</title>

    <script type="text/javascript">
        var Ext = Ext || {};
            Ext.repoDevMode = true;

        Ext.beforeLoad = function(tags){
            var theme = location.href.match(/theme=([\w-]+)/), //这里采用获取地址参数来加载语言,可以根据需求改为从cookies获取语言
            locale = location.href.match(/locale=([\w-]+)/);
            theme  = (theme && theme[1]) || 'crisp';
            locale = (locale && locale[1]) || 'en';
            Ext.manifest = theme + "-" + locale;
         };
    </script>

    <!-- The line below must be kept intact for Sencha Cmd to build your application -->
    <script id="microloader" type="text/javascript" src="bootstrap.js"></script>
</head><body></body></html> 
```

*_注意: index.html里的注释,根据需求修改_*

这样B部分的国际化就完成了,每次加载页面时根据cookies里的不同参数Ext JS组件就能显示不同语言.你应该注意到了这个方案不仅能改变语言,还能改变主题.此方案默认采用的是Ext JS5的crisp主题,如果你是低版本且没有这个主题的项目可以修改为你项目对应的主题.

###不足

因为A部分方案是不需要刷新页面就能切换语言的,但B部分的方案需要刷新页面来支持切换,所以整个项目还是需要刷新来加载语言.解决办法是将B部分里的翻译集成到A里,或者还有其他的办法,欢迎赐教.

###最终效果展示

![图片4][img4]

![图片5][img5]


相关文档:

[http://docs.sencha.com/extjs/6.0.2-classic/guides/core_concepts/localization.html](http://docs.sencha.com/extjs/6.0.2-classic/guides/core_concepts/localization.html)

[https://www.sencha.com/blog/internationalization-localization-with-sencha-ext-js/](https://www.sencha.com/blog/internationalization-localization-with-sencha-ext-js/)

[http://extjs.eu/localization-of-ext-applications/](http://extjs.eu/localization-of-ext-applications/)

[https://www.sencha.com/forum/showthread.php?295987-UPDATED-Two-themes-in-single-app-5.1](https://www.sencha.com/forum/showthread.php?295987-UPDATED-Two-themes-in-single-app-5.1)

[img1]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/jnesis-i18n-img1.png
[img2]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/jnesis-i18n-img2.png
[img3]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/i18n-img3.png
[img4]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/i18n-img4.png
[img5]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/i18n-img5.png
