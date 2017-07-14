# FamilyTreeView
### 重大改进，增加仿“亲友+”App的家谱，与之前的没有任何关联，可以直接翻到第二部分“仿亲友+家谱”部分进行阅读
### 家谱树绘制Demo，主要使用自定义ViewGroup，和使用canvas进行划线，现阶段实现了自己、配偶、兄弟姐妹、父母、祖父母、外祖父母、子女和儿媳妇，女婿以及孙子，共五代的绘制，加入了touch事件可以移动，代码可能写得相对比较死，基本只能使用于家谱展示。还增加了有养父母的控件，其实是拷贝原来的改的。

## 第一部分：简易家谱
## 运行流程

#### 1、首先通过assets获取Json
![assets](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/QQ%E6%88%AA%E5%9B%BE20170602231531.png)

部分Json
![Json](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/QQ%E6%88%AA%E5%9B%BE20170602231651.png)

#### 2、将Json转成对象列表
![对象](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/QQ%E6%88%AA%E5%9B%BE20170602232244.png)

#### 3、将对象列表存进数据库
#### 4、查询数据库拼装带关系的对象
#### 5、将关系对象交给控件让控件绘制关系图

## 预览图

![静态预览图](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/device-2017-06-23-155317.png)

![静态预览图](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/device-2017-06-23-155343.png)


## 思路说明
#### 1、代码执行流程
	1.1、首先当然时初始化数据，包括家庭成员对象、画笔等等。
	1.2、然后就是根据家庭成员对象，测量布局后ViewGroup的宽高，最大高度是固定的最大一共五代人，宽度分别计算每一代的最大宽度来得出。
	1.3、初始化View，把家庭成员的View都初始化。然后调用invalidate()更新界面。
	1.4、在onMeasure测量家庭成员View的宽高。
	1.5、在onLayout里编排好每个成员的具体位置。
	1.6、在onDraw方法里，利用canvas画出家庭成员关系的连线。
	1.7、最后在onTouchEvent方法写下触摸事件移动ViewGroup，在onInterceptTouchEvent写下移动时是否拦截成员View的点击事件。
  
#### 2、onLayout方法的定位处理
	2.1、首先订好了自己的View的位置,自己的位置位于整个布局的中心。
	2.2、如果有配偶则将配偶排在自己的右边。
	2.3、如果有兄弟姐妹那么将兄弟姐妹按序一字排在自己的左边。
	2.4、如果只有父亲或父母，则排在自己正上方，如果双亲都有，那么父亲排在自己左上方，母亲排在自己右上方。
	2.5、祖父母和外祖父母的原理和父母的一样。不过祖父母是相对父亲来排，而外祖父母是相对母亲来排。
	2.6、这里要先排孙子孙女，因为我的孙子孙女的位置决定了我的子女的位置。
	2.7、排好孙子孙女，就按照孙子孙女的位置确定子女的位置。
	
#### 3、onDraw方法的画线处理
	3.1、首先是自己和配偶的连线，都是canvas，Paint，Path的功夫，不懂就百度。
	3.2、然后是父母以及祖父母的连线，这里并不难我就不多说了，代码写得很清楚了。
	3.3、接着就是兄弟姐妹的连线，直接从最左边兄弟的X坐标，练到自己View的X坐标，这里是偷懒的连线方式。(￣▽￣)")
	3.4、最后就是子女和孙子女的画线，这里也是先连孙子女，把同父母的孙子孙女先连线，然后再连孙子女和子女的线，接着把子女连起来，最后就是我和子女的连线。

	
	
## 第二部分：仿亲友+App的家谱
### 说明：这部分功能是参照亲友+App来做的，UI和亲友+App基本相似，逻辑应该不同，因为一开始是想反编译亲友+来看看人家是怎么做的，可惜亲友+用了360加固，我没时间去搞去壳，所以就自己琢磨逻辑。显示的内容是在亲友+的基础上多加了底层家庭成员的配偶显示。你也可以通过设置mBottomNeedSpouse变量来设置是否显示底层家庭成员的配偶。具体效果自己运行App看一下。

## 运行流程

#### 1、首先通过assets获取Json
#### 2、将Json转成对象列表
#### 3、将对象列表存进数据库
#### 4、将选中的加入的ID传入控件里，控件会查询数据库拼装带关系的model对象，然后通过对象绘制关系图
![对象](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/%E4%BB%BF%E4%BA%B2%E5%8F%8B%2BModel.png)

## 预览图，这里就放一张意思意思，因为关系图太庞大，具体效果自己运行App体验吧

![静态预览图](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/%E4%BB%BF%E4%BA%B2%E5%8F%8B%2B%E9%A2%84%E8%A7%88%E5%9B%BE.png)

## 思路说明
#### 1、代码执行流程
	1.1、首先当然时初始化数据，包括家庭成员对象、画笔等等。
	1.2、然后就是根据传进来的家庭成员ID，进行一系列的数据库查询，分边装在不同的model或不同的list里。
	1.3、初始化View，把家庭成员的View都初始化。初始化的时候已经定好了该view的位置。
	1.4、在onMeasure测量家庭成员View的宽高。
	1.5、在onLayout里编排好每个成员的具体位置。
	1.6、在onDraw方法里，利用canvas画出家庭成员关系的连线。
	1.7、最后在onTouchEvent方法写下触摸事件移动ViewGroup，在onInterceptTouchEvent写下移动时是否拦截成员View的点击事件。
  
#### 2、定位流程
![结构图](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/%E7%BB%93%E6%9E%84%E5%9B%BE.png)
	定位遵循由底层一层一层上去，因为子层位置确定了那么父层的位置就确定了。
	流程顺序是：(1)(2) → (3) → (4)(5) → (6)(7) → (8)(9)(10) → (11)(12) → (13)(14)，具体逻辑可以看initView()方法
	由于(1)(2)和(4)(5)和(11)(12)的定位逻辑是相似的，还有(6)(7)和(13)(14)也是相似的，所以就分别写成了相同的方法，然后传入是第几代，和数据源就可以定位了。
	然后我和配偶(3)的定位写一个方法，因为存在特殊性；还有父母、祖父母、外祖父母作为一个整体来写一个方法来定位，也是因为其特殊性。
	
#### 3、画线流程
	1、把每个和该view有关系的线画上：(1)(2) → (4)(5) → (6)(7) → (11)(12) → (13)(14) → (3) → (8) → (9) → (10)
	2、把每部分有关联的线画上：我的子女之间的连线 → 我亲兄弟姐妹和我之间的连线 → 祖父母和父亲的连线 → 外祖父母和母亲的连线 → 父亲和其兄弟姐妹之间的连线 → 母亲和其兄弟姐妹之间的连线。具体是怎样我也不好描述，但是代码里已经分好了每个步骤，所以可以去看代码理解。
	
	
### Github：[https://github.com/ssj64260/FamilyTreeView](https://github.com/ssj64260/FamilyTreeView)



















