---
title: 使用Python实现伪防沉迷工具
date: 2020-04-08 10:27:19
tags:   [python, 小项目]
---
<!--<script src="https://cdn.bootcss.com/highlight.js/10.0.0/highlight.min.js"></script>-->
# __· 背景和需求__  
　　因为疫情期间嘛，老爹也只能开网课，线上教棋了，但是网课又不开摄像头的那种，根本不知道学生在干嘛，这不就有了一个学生在那表面上着网课实际上是在那玩游戏，被他妈妈逮了个正着...因此我老爹就想看看能不能上网课的时候让他只能用上网课的那个程序，而正好我在看python的psutil的模块，就尝试着帮我老爹写一个看看喽  
<!--more-->  
# __· 分析__
　　1. 首先我需要检测正在运行进程，之后找到在封禁名单内的主进程，之后shutdown就可，这部分psutil模块就可以实现  
　　2. 作为一个程序，我还是得写一个ui界面的，本来是想用tkinter的，不过Python这个标准GUI库功能确实不咋地，这里就现学了一下wxpython来作为程序的ui界面
　　3. ui界面这里主要是要提供一个最小化到托盘以及推出功能，不然的话现在的这些家长不太知道怎么退出就比较头疼了
# __· 代码实现__
## 1. 主逻辑部分  
　　· 这里我们把主逻辑部分写到一个线程里，为此我们需要线程模块，这里从`threading模块`中导入`Thread类`，之后定义我们自己的线程类，继承自Thread，主逻辑放到`重写的run()方法`中
　　· 正式开始主逻辑部分，这里需要被限制程序的主进程名，为了以后好扩展，这里我们将这些主进程名放到一个列表里，之后通过转换为json格式存储到文件中，为此我写了一个config.py用来实现该功能

```python
import json

# 存储限制进程列表到文件中去
def main():
    try:
        f = open('learn_tool.conf', 'w', encoding='utf-8')
        limit_process_list = ['DouyuLive.exe', 'QQGame.exe', 'MicrosoftEdge.exe', '360se.exe', '360chrome.exe',
                              'firefox.exe', 'chrome.exe', 'QQBrowser.exe', 'SogouExplorer.exe']
        json_list = json.dumps(limit_process_list)
        f.write(json_list)
    except LookupError:
        print('指定了未知编码！')
    except IOError as ex:
        print(ex)
        print('写文件时发生错误！')
    finally:
        f.close()

    print('操作成功！')


if __name__ == "__main__":
    main()
```

　　· 运行config.py就会生成存放限制进程名的配置文件：learn_tool.conf，之后也可以直接去这个文件中增添其他要限制的进程（ps：本来是应该在ui界面里加入这个增添限制进程功能的，有点懒，先放着吧<img src="/使用Python实现伪防沉迷工具/挑眉.gif" width=27 height=27>）

　　· 之后在类中写一个前面的run()方法中打开该文件，将其从json格式转为list即可；关键在之后：这里直接设置死循环，因为要不断的检测是否有被限制进程，使用`psutil.pids()`__获取所有当前正在运行的进程id的列表__，之后for循环遍历，通过`psutil.Process(pid).name()`来__获取该pid对应的进程名__，之后判断进程名是否在限制进程的list中即可：不在，则继续循环；在，则调用`terminate()`方法来__关闭进程__
```python
# 自定义限制进程
class AntiInsertion(Thread):
    def run(self):
        # 获取要限制的进程列表
        limit_process_list = self.read_conf('learn_tool.conf')

        while True:
            # 存放正在运行的限制进程的信息
            now_run_limit_process_list = []
            # 获取当前正在运行的所有进程的pid
            try:
                now_pids = psutil.pids()       # 当前所有进程pid的list
                for pid in now_pids:
                    if pid in psutil.pids():
                        _p = psutil.Process(pid)    # 获取每个进程
                        if _p.name() in limit_process_list:
                            now_run_limit_process_list.append(_p)

                for i in now_run_limit_process_list:
                    if i.pid in psutil.pids():
                        log.my_log(1, str(i.pid) + "：" + i.name())
                        i.terminate()
                    else:
                        log.my_log(2, i.name() + " not exist")
            except Exception as error:
                log.my_log(2, error)

    # 读取配置文件信息
    def read_conf(self, filename) -> list:
        try:
            conf_f = open(filename, 'r', encoding='utf-8')
            limit_process_list = json.loads(conf_f.read())
        except FileNotFoundError:
            print('系统配置文件未找到！')
        except LookupError:
            print('指定了未知编码！')
        except UnicodeDecodeError:
            print('读取文件时解码错误！')

        return limit_process_list
```


　　· 上面代码中的log为我__自定义的日志记录模块__:log.py，用来将每次_关闭进程的信息_及_错误信息_存入到log.txt文件中
```python
import logging


def my_log(model: int, message: str):
    # 设置logging模式
    logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', filename='./log/log.txt', filemode='a')
    logger = logging.getLogger(__name__)
    if model == 1:
        logger.info(message)
    elif model == 2:
        logger.warning(message)


if __name__ == '__main__':
    # 测试数据
    my_log(1, '1111')
    my_log(2, '2222')
```

## 2· ui部分设计
　　· 这里选择了__wxpython__来进行ui设计，需要定义一个自己的__Frame类__，从`wxpython继承`，在`__init__()`中初始化窗口大小、窗口位置、ui背景颜色、托盘以及各种控件等，不过这里我们并不需要什么控件，只需要实现__托盘__和__右击托盘退出__功能
```python
import wx
import sys
import win32api


class MyFrame(wx.Frame):
    # 程序主窗体类，从wx.Frame继承

    def __init__(self):
        wx.Frame.__init__(self, None, -1, APP_TITLE, style=wx.DEFAULT_FRAME_STYLE ^ wx.RESIZE_BORDER)
        # 给Frame增添托盘，定义在下面
        self.taskBarIcon = MyTaskBarIcon(self)

        self.SetBackgroundColour(wx.Colour(224, 224, 224))
        self.SetSize((400, 400))
        self.Center()

        # 下面代码用于处理图标
        if hasattr(sys, 'frozen') and getattr(sys, 'frozen') == "windows_exe":
            exe_name = win32api.GetModuleFileName(win32api.GetModuleHandle(None))
            icon = wx.Icon(exe_name, wx.BITMAP_TYPE_ICO)
        else:
            icon = wx.Icon(APP_ICON, wx.BITMAP_TYPE_ICO)
        self.SetIcon(icon)

        # 给gui窗体上的关闭按钮绑定隐藏到托盘事件
        self.Bind(wx.EVT_CLOSE, self.OnHide)

        # 如果需要添加扩展控件，写在__init__()中即可
        pass
    
    # 定义函数用于隐藏程序到托盘
    def OnHide(self, event):
        self.Hide()
```

　　· 之后我们来实现ui界面的关键：__托盘的实现__，这里要用到`wx.adv`，依旧定义我们自己的托盘类，继承自`wx.adv.TaskBarIcon`；在`__init__()`中__初始化Frame、图标设置等__，创建菜单需要重写`CreatePopupMenu()`方法，在该方法中我们需要使用`wx.Menu()`来__创建菜单__，之后通过`wx.Menu.Append()`方法__将菜单项添加到菜单中__，这里每个菜单项需要用`wx.MenuItem()`来创建，而__每个菜单项的点击事件的绑定__则使用`Bind()`来完成，完成之后`return menu`
```python
# 托盘类
class MyTaskBarIcon(wx.adv.TaskBarIcon):
    def __init__(self, frame):
        wx.adv.TaskBarIcon.__init__(self)
        self.Frame = frame
        self.SetIcon(wx.Icon(APP_ICON), APP_TITLE)

    # 获取Menu数据
    def set_menuitem_data(self):
        data_list = (('About', self.on_about), ('Show', self.on_show), ('Close', self.on_close))
        return data_list

    # 创建菜单
    def CreatePopupMenu(self):
        my_menu = wx.Menu()
        for itemName, itemHolder in self.set_menuitem_data():
            if not itemName:
                my_menu.AppendSeparator()
                continue

            # 创建每一个菜单项
            menu_item = wx.MenuItem(None, wx.ID_ANY, text=itemName, kind=wx.ITEM_NORMAL)
            my_menu.Append(menu_item)
            self.Bind(wx.EVT_MENU, itemHolder, menu_item)

        return my_menu

    # 显示app相关信息，这里加上作者信息吧还是，2333
    def on_about(self, event):
        wx.MessageBox('signed by sophistic-cat', '关于')

    # 显示主菜单
    def on_show(self, event):
        '''
        # 这里本来设计是用来显示主窗体的，但为了避免小孩子们通过任务管理器关闭，就把这里注释掉了
        if self.Frame.IsIconized():
            self.Frame.Iconize(False)
        if not self.Frame.IsShown():
            self.Frame.Show(True)
        self.Frame.Raise()
        self.Frame.Maximize(True)   # 最大化显示
        '''
        pass

    # 退出app
    def on_close(self, event):
        # 实例化了一个自定义的弹窗类
        myED = ExitDialog()
        # 显示弹窗
        myED.ShowModal()
        # 判断密码是否正确，正确则退出
        # 这里是类中有一个flag标志位，当密码正确时会将flag设置为true，通过get_flag()返回
        if(myED.get_flag()):
            wx.Exit()
```

　　· 之后我们来定义上面的__退出弹窗__，依旧写一个自己的__弹窗类__，继承自`wx.Dialog`，和wx.Frame类似，在`__init__()`中进行初始化、拜访控件位置、设置弹窗大小位置等，这里主要是要实现给Button绑定事件和提取TextCtrl中的内容；__绑定事件__很简单，依旧是用`Bind`；而__提取TextCtrl中的输入内容__也有现成的方法，`调用TextCtrl的GetValue()方法`即可。对了，这里我们将退出密码依旧以文件形式保存，然后本来想做个加密算法的，不过，emmmm，懒嘛，就先明文保存到文件吧，哎嘿嘿<img src="/使用Python实现伪防沉迷工具/哎嘿嘿.jpg" width=27 height=27>
```python
# 定义退出时输入密码的Dialog
class ExitDialog(wx.Dialog):
    def __init__(self):
        wx.Dialog.__init__(self, None, -1, "Exit_verify", size=(300, 100))
        # 设置静态文字，提示后面输入密码
        self.static_text = wx.StaticText(self, -1, u'请输入密码：', pos=(10, 20), size=(80, -1), style=wx.ALIGN_RIGHT)
        # 设置输入框
        self.Exit_Pwd = wx.TextCtrl(self, -1, '', pos=(90, 15), size=(100, -1), style=wx.TE_PASSWORD)
        # 设置退出按钮
        self.Sure_Bt = wx.Button(self, -1, '确认', pos=(200, 12), size=(50, -1))

        self.flag = False
        self.Center()
        # 给按钮绑定“检查密码”点击事件
        self.Sure_Bt.Bind(wx.EVT_BUTTON, self.checked_pwd)

    def checked_pwd(self, evt):
        # 获取输入框的文字
        content = self.Exit_Pwd.GetValue()
        # 打开密码文件，判断是否相同
        with open("./data/pwd.txt", "r") as f:
            date = f.read()
            if content == date:
                # 密码一致，将标志位flag设置为True
                self.flag = True
                self.Destroy()
            else:
                # 密码不一致，弹出密码错误的提示框
                wx.MessageBox('密码错误！', 'Error')
    
    # 用于外部获取flag值，来判断密码输入是否正确
    def get_flag(self):
        return self.flag
```

　　· 至此基本功能、ui设计完毕，不过为了再玩一下wxpython，就又添加了一个__启动界面__，我们这里新建一个函数用来创建启动界面，两行代码就可实现，关键在于`wx.adv.SplashScreen()`的使用
```python
def create_splash():
    # 创建开始界面
    screen = wx.Image(r'./pic/welcome.jpg').ConvertToBitmap()   # 选择一张图片（bitmap格式）来做启动界面
    
    '''这里注意下参数含义:
        第一个参数是bitmap图；
        第二个参数是格式，这里设置为居中在屏幕中间并停顿一段时间；
        第三个参数是设置停顿时间，2000即为2s
        第四个参数是parent，这里没有，设置为None即可
    '''
    wx.adv.SplashScreen(screen, wx.adv.SPLASH_CENTRE_ON_SCREEN | wx.adv.SPLASH_TIMEOUT, 2000, None, -1)
```

　　·　之后我们要新建自己的__主程序类__，继承自`wx.App`，在这里重写`OnInit()`方法（这是主程序入口类），将其Frame设置为我们的Frame实例即可
```python
class MainApp(wx.App):
    Frame = None

    # 主程序入口类
    def OnInit(self):
        self.SetAppName(APP_TITLE)
        self.Frame = MyFrame()
        # self.Frame.Show()     # 设置启动时显示主窗体
        return True
```

　　· 最后，创建我们主程序实例，启动我们的__主逻辑进程__并设置为__守护进程__（这样的话，当我们退出时就会关闭该进程），启动开始界面，mainloop我们的主程序
```python
if __name__ == "__main__":
    app = MainApp(redirect=True, filename="./log/log_debug.txt")
    AntiInsertion(daemon=True).start()
    create_splash()
    app.MainLoop()
```

## 3. 代码打包问题
　　· 这里__打包__使用的是__pyinstall__，我的整个程序目录如下，打包则在main.py文件所在位置打开cmd进行打包，打包代码如下：
　　　　参数说明：__-D__：__除了主程序外还会在dist文件夹下生成很多依赖文件__
　　　　　　　　　__-i__：__选择.exe的图标文件__
　　　　　　　　　__-noconsole__：__去除cmd黑框__
　　__程序目录__： <img src="/使用Python实现伪防沉迷工具/程序目录.png" style="right:1000px">
　　
　　__打包命令__： `pyinstall -D main.py -i ./pic/app_icon.ico -noconsole`  
　　
　　· 生成的程序在dist目录下；但是有__一个大问题__：__程序所需的/data、/log、/pic等文件都没有打包进去__，emmm，所以直接运行会报错，当你把main.exe依赖的文件复制进去之后就ok了

## 程序演示
<img src="/使用Python实现伪防沉迷工具/程序测试.gif">