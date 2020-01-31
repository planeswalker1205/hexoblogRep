# IDEA Intellj

**一**.打开setting窗口，Settings分为两个部分，分别是Template Project Settings和IDE Settings。

* Template Project Settings 是针对每个项目，不同的项目的配置都不一样。
* IDE Settings 是IDE配置，所有项目的配置都一样

1.显示行号:Settings->Editor->Appearance,勾上"Show line numbers"

2.取消拼写检查，打开Setting->Inspection，取消“Spelling”

3.关闭自动保存，打开Settings->General，反选“Synchronize file on frame activation”和“Save files on frame deactivation”，同时修改未保存的显示星号，打开Setting-Editor->Editor Tabs,勾上“Mark modified tabs with aserisk

4.修改属性资源文件(.properties)的编码，打开Setting->File Encoding,设置Properties File 的编码为UTF-8，并勾上"Transparent native-to-ascii conversion

5.修改代码提示快捷键与输入法快捷键冲突的情况。打开Setting-keymaps"，展开下拉列表Main menu->Code->Completion,修改Basic和SmartType快捷键为个人喜好。

6.隐藏没用到的文件，比如 IDEA 的项目配置文件（*.iml 和*.idea），打开 Settings-File Types， 加入要隐藏的文件后缀。 

二.IDEA快捷键

1. ctrl+space 基本代码不全
2. ctrl+shift+space  智能代码补全，列出与预期类型一致的方法或变量
3. ctrl+shift+enter 补全语句
4. ctrl+p 显示方法参数
5. ctrl+q 显示注释文档
6. shift+f1 显示外部文档
7. alt+insert 生成代码，生成getter。setter。构造器等
8. ctrl+o 重写父类方法
9. ctrl+i  实现接口方法
10. ctrl+alt+t
11. ctrl+/  //注释或取消
12. ctrl+shift+/     /** **/注释或取消
13. ctrl+w   选择代码块，连续按会增加选择外层代码块
14. ctrl+shift+w   取消代码块，连续按会增加到内层
15. alt+q   显示类描述信息
16. alt+enter-fixes   显示快速修复列表
17. ctrl+alt+L  格式化代码
18. ctrl+alt+i  自动优化代码锁紧
19. ctrl+d  重复代码
20. ctrl+y  删除行









