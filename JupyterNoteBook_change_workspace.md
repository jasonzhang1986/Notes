Window 下修改 Jupyter Notebook 的工作路径

在 cmd 中，​输入“ipython notebook ”或 “jupyter notebook” 打开 notebook，此时 cmd 的当前路径即为 notebook 的工作路径。

另外，可通过设置 config 文件的方法来设置固定的工作路径。

方法是：

1. 选择一个用于作为工作空间的文件夹
2. 在 cmd 中进入该文件夹的路径
3. 在cmd中 输入​命令 jupyter notebook --generate-config
4. 此时便生成一个notebook的config文件​，文件名是“jupyter_notebook_config.py”路径是C:\Users\用户名.jupyter\jupyter_notebook_config.py
5. 打开该文件，修改路径
```
# The directory to use for notebooks and kernels.下面的
# c.NotebookApp.notebook_dir = ””​为
c.NotebookApp.notebook_dir = "D:/Jupyter"（注意将#号删除）
```
