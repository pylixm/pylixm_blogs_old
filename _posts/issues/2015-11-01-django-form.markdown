---
layout : post
title : django form表单的数据 select 值更新问题
category : Q&A
tagline: ""
date : 2015-11-01
tags : [q&a, django,]
---



### Q & A：

今天在项目中遇到一个问题，有个django增删改查的模板页面,其中的编辑页面使用自定的form来构建的显示内容。表单的数据是从数据库中查询出来展示的，当修改数据库的内容后，form的展示的信息并没有修改。当重启后，form的数据重新加载。查了许多资料，都没有对form表单数据的加载时间的解答。
从现象来看，form的数据的加载时在服务启动时，就加载了。代码如下：

view.py:
{% highlight python linenos%}
def testform(req):
    form = testForms.testForm()
    print '>>>',form
    return render_to_response("test.html",{'form':form})
{% endhighlight%}

urls.py
{% highlight python linenos%}
# form测试
url(r'^test/form/$','manager.makoViews.testform'),
{% endhighlight %}

models.py
{% highlight python linenos%}
from django import forms
import models

class testForm(forms.Form):
    device_types = models.Role.objects.all().values_list('id','name')
    name = forms.CharField(widget=forms.widgets.Select(choices=device_types))
{% endhighlight %}

html:
{% highlight html linenos%}
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<table>
{{form}}
</table>
</body>
</html>
{% endhighlight %}

### 解决方案：

models 代码为：
{% highlight python linenos%}
name2= forms.ModelChoiceField(label=u'name2',queryset=models.Role.objects.all(),to_field_name="id") 
{% endhighlight %}

默认情况下，在页面中生成的select的 option的value是queryset的key，而值是queryset模型中的__unicode__方法返回的值。
可以使用``to_field_name`` 来制定option的value。

