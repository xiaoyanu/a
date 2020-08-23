# 1.简介

Custom Main Menu 是一个能够自定义主界面的模组，需要配合 Resource Loader 模组使用。
采用json语法写的配置文件，可以热加载出自定义的样式。使用方式颇有点像CSS样表。许多著名的国外整合包无一例外都采用了这个模组。效果非常好，这里拿几个作者设计的样式给大家看看吧。（作者美工水平不咋行啊）

# 2.文件结构

配置文件为游戏主文件夹下`config\CustomMainMenu\mainmenu.json`文件； 所有的图片文件地址都在游戏主文件夹下`resources\mainmenu`文件夹下。
目前这货好像还不支持UFT-8编码，所以还是别用中文了吧【还看着干什么，快去github轰炸作者吧，这作者是我见过最偷懒的作者】。
修改好了之后，在主界面按下`ctrl+r`键即可重载配置。

配置文件结构一般如下

```
{
"images": {
        "图片一": {
        },
        "图片二": {
        }
    },

"buttons": {
        "按钮一": {      
        },
        "按钮二": {      
        }
    },

"labels": {
        "文本一": {      
        },
        "文本一": {      
        }
    },

"other":{
        "background":{          
        }
    }
}
```

# 3.背景的添加

背景分为动态背景和静态背景。

## （1）静态背景：

```
{
"other":{
        "background":{
            "image":"mainmenu:001.png",
            "mode":"fill"
        }
    }
}
```

如案例所示，背景属于”other”分区，有两个参数可选：

- image：【可选】标定着背景图片的地址，注意这个地址写法比较特殊。图片只支持png格式。
- mode：【可选】图片的填充模式，有`fill`,`stretch`,`center`,`tile`四个选项可选。
  - fill：适应，保证图片长宽比前提下，尽可能铺满屏幕。会适当裁剪图片。
  - stretch：拉伸，尽可能铺满屏幕，如果不合适会强制拉伸图片。
  - center：居中，不改变图片大小和比例，依据屏幕大小裁剪图片。
  - tile：平铺，重复图片铺满屏幕。

## （2）动态背景

```
{
"other":{
    "background":{
            "image" : "",
            "slideshow":{
                "images" : ["mainmenu:001.png","mainmenu:002.png","mainmenu:003.png"],
                "displayDuration" : 100,
                "fadeDuration" : 40
            }
        }
    }
}
```

- image：此时为空，但是不可略去不写。
- slideshow：添加循环的图片背景。
  - images：添加循环的图片地址。
  - displayDuration：图片停留时间，单位为tick。
  - fadeDuration：转换图片时间，单位为tick。

# 4. 文字的添加

网络加载文字：

```
{
"labels": {
    "changelog":{
          "text":"web:http://pastebin.com/raw.php?i=MmSCr6zV",
          "posX" : 2,
          "posY" : 0,
          "color" : -1,
          "alignment" : "left_center"
        }
    }
}
```

硬编码文字加载【不支持中文哦，会乱码的】

```
{
"labels": {
      "mojang": {
          "text" : "Copyright Mojang AB. Do not distribute!",
          "posX" : -197,
          "posY" : -10,
          "color" : -1,
          "alignment" : "bottom_right"
        }
    }
}
```

- text：文字，可选网络加载文字，加载诸如更新日志一类的东西。文字中可以直接内镶Minecraft原版支持的颜色代码，还可以直接调用语言文件中的key加载文字。文字中还可以使用占位符太指代特定数据，占位符有如下几种：
  - \#mcversion#：MC的版本
  - \#fmlversion#：FML的版本
  - \#mcpversion#：MCP的版本
  - \#forgeversion#：Forge的版本
  - \#modsloaded#：mod加载数量
  - \#modsactive#：mod激活数量
  - \#time#：时间
  - \#username#：当前游戏ID
  - \#date#：当前日期（调取的系统时间）
- posX, posY：文字定位。如果不设定alignment参数，默认中心对齐。
- color：颜色代码，采用整型数表示的RGB码。
- alignment：对齐方式。有`bottom_left`,`bottom_right`,`top_left`,`top_right`,`button`,`center`,`top_center`,`left_center`,`bottom_center`,`right_center`。

除此之外还有几个属性可以选择添加：

- hoverColor：鼠标悬浮上去的颜色。
- hoverText：鼠标指针悬浮上去的文字。
- anchor：可以传入`start`，`middle`，`end`三个参数，分别对应左、中、右对齐。
- action：用户点击文字之后会发生的行为，和按钮添加部分的action用法一致，此处不再细说。
- fontSize：字体的大小。

推荐两个使用的颜色代码网站：

- [Color Hex Color Codes](http://www.color-hex.com/)：能够查询颜色的RGB码等诸多信息。
- [Calculate RGB Int](https://www.shodor.org/stella2java/rgbint.html)：可以将RGB码专为10进制。

# 5.图片的添加

图片只能用png格式。

```
{
    "images":{
        "picture":{
            "image":"mainmenu:001.png",
            "posX":-120,
            "posY":-120,
            "width":240,
            "height":240,
            "alignment":"center"
        }
    }
}
```

- image：图片地址。
- posX, posY：图片坐标，定位点为图片左上角。
- width, height：图片大小，过大的部分会直接裁减掉。
- alignment：对齐方式

除此之外还可以添加：

- hoverImage：鼠标指针悬浮上去的图片。
- slideshow：和之前讲过的slideshow用法一致，此处不再累述。

推荐一个好用的，制作minecraft样式文字图标的网站：[textcraft](https://textcraft.net/)。

# 6.按钮的添加

```
{
"buttons":{
        "singleplayer":{
            "text":"menu.singleplayer",
            "texture":"mainmenu:shortbutton.png",
            "posX":-100,
            "posY":-8,
            "width":98,
            "height":20,
            "imageWidth":98,
            "imageHeight":20,
            "alignment":"center",
            "action":{
                "type":"openGui",
                "gui":"singleplayer"
            }
        },
        "ftbForums":{
            "text":"FTB Forums",
            "texture":"mainmenu:shortbutton.png",
            "posX":2,
            "posY":58,
            "width":98,
            "height":20,
            "imageWidth":98,
            "imageHeight":20,
            "alignment":"center",
            "action":{
                "type":"openLink",
                "link":"https://forum.feed-the-beast.com/forum/"
            }
        }
    }
}
```

- text：按钮上的文字，可以直接调用lang文件中的key，或者是直接写上文字（不支持中文，会乱码）。
- texture：按钮背景图片。
- posX, posY：按钮图片坐标，定位点为图片左上角。
- width, height：按钮图片大小，过大的部分会直接裁减掉。
- alignment：对齐方式。

除此之外还可以添加这几个参数

- hoverText：鼠标悬浮时候显示的文字。
- normalTextColor：文字颜色。
- hoverTextColor：鼠标悬浮时候显示的文字颜色。
- textOffsetX， textOffsetY：按钮文字的对齐数值。
- tooltip：显示文本提示。

当然，还有我们最重要的action参数，action下必须要有type参数，可以传入`openLink, openGui, quit, refresh, connectToServer, openFolder`。

- openLink：打开一个链接。

  ```
  "action":{
        "type":"openLink",
        "link":"https://www.feed-the-beast.com"
  }
  ```

- openGui：打开特定的GUI

  ```
  "action":{
          "type":"openGui",
          "gui":"languages"
  }
  ```

  gui参数里可以传入的数据有（自己猜意思吧，懒得翻译了）

  ```
    mods
    singleplayer
    singleplayer.createworld
    multiplayer
    options
    languages
    options.ressourcepacks
    options.snooper
    options.sounds
    options.video
    options.controls
    options.multiplayer
  ```

- connectToServer：连接的服务器，需要写入一个服务器ip。

  ```
  "action":{
        "type":"connectToServer",
        "ip":"127.0.0.1"
  }
  ```

- openFolder：打开特定文件夹，需要注意的是默认的主目录是游戏主文件夹。

  ```
  "action":{
        "type":"openFolder",
        "folderName":"config"
  }
  ```

- quit和refresh参数，没有特定其他属性需要添加。

## About

关于萌生建立博客的想法，最早来自于不喜欢贴吧或者mcbbs的排版，而且平时空闲时间也喜欢写一些乱七八糟的东西，亦或是翻译一些wiki。

本来是打算用烂大街的wordpress的，然而它提供的免费版本不能改插件，想要更高的自定义功能需要自己去弄主机并且映射域名。如果算域名主机的花费，一个月恐怕就得花60大洋。然而我只是个穷学生啊。

之后JJN告诉我可以用jekyll结合github建立免费的静态博客的，套用[模板](https://github.com/qwtel/qwtel.github.io)以后就是你们现在看到的这个界面了

说实话一直对网页搭建很感兴趣，之前曾经看过黄油的博客，觉得在这种快节奏的网络生活中留下一份自己的宁静也是极好的，很受感动。

之后又看了JJN和Towdium的博客，觉得跃跃欲试，最后终于是开工了。虽然有太多不足，太多缺陷，打这些字都觉得自己语言功底已经近乎为零。所以还请那些看到这个破网站的人还请多多指教。