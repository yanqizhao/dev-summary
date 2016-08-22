# JSPatch问题

## 注意getter方法为isXXX的属性
如UISwtich中有一个属性为on，getter方法为isOn，在JSPatch中调用其get、set方法时应使用isOn()与setOn()



