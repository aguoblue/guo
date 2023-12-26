
# day1
###### 轴心
local
global
###### 父子关系


- 坐标 transform
```csharp
Debug.Log("start");
//当前物体
GameObject obj = this.gameObject;
string name = obj.name;
Debug.Log("name: "+name);
//坐标
Transform tr = obj.transform;
Vector3 pos =  tr.position;
Debug.Log("** pos: " + pos.ToString("F3"));

//世界坐标
Vector3 pos1 = this.gameObject.transform.position;
//本地坐标  this.gameObject.transform 简化 this.transform
Vector3 pos2 = this.gameObject.transform.localPosition;

Debug.Log("** pos1: " + pos1.ToString("F3"));
Debug.Log("** local pos2: " + pos2.ToString("F3"));
```


- 帧更新
```csharp
//帧更新
Debug.Log("** update time " + Time.time);
Debug.Log("** update deltaTime " + Time.deltaTime);
```

- 物体移动 复制或者Translate
```csharp
//物体移动
Vector3 pos = transform.localPosition;
pos.x += 0.1F;
this.transform.localPosition = pos;
//匀速
float speed = 3;
float distance =  speed *  Time.deltaTime;
Vector3 pos = transform.localPosition;
pos.x += distance;
this.transform.localPosition = pos;
//另一种变换坐标
this.transform.Translate(distance, 0, 0);
//相对运动
//this.transform.Translate(distance, 0, 0,Space.World);//按照世界坐标系
//this.transform.Translate(distance, 0, 0, Space.Self);//按照local坐标系
```
- 转向 transform.LookAt
```csharp
//获取目标物体
GameObject obj = GameObject.Find("Sphere1");
//转向
this.transform.LookAt(obj.transform);
//前进
this.transform.Translate(0, 0, distance, Space.Self);
```
- 两个物体间的距离 Vector3.magnitude
```csharp
Vector3 pos1 = this.gameObject.transform.position;
GameObject flag = GameObject.Find("Sphere1");
Vector3 pos3 = flag.transform.position;
Vector3 p = pos3 - pos1;
float distance = p.magnitude;
Debug.Log("*** distance: "+ distance.ToString("F3"));
```
edit 下有一个 lock view to selected 选择某个物体的视角作为场景视角
- 物体旋转  transform.localEulerAngles
```csharp
//旋转
float rotateSpeed = 30;
Vector3 angles = this.transform.localEulerAngles;
angles.y += rotateSpeed * Time.deltaTime;
this.transform.localEulerAngles = angles;

//另一种
//transform.Rotate(dx,dy,dz,space);
this.transform.Rotate(0, rotateSpeed * Time.deltaTime, 0,Space.Self);
```
\- transform.localEulerAngles 物体在其父物体局部坐标系中的旋转角度。
\- transform.eulerAngles 物体在世界坐标系中的旋转角度。
- 相对旋转 transform.Rotate
```cs
//自转
this.transform.Rotate(0, rotateSpeed * Time.deltaTime, 0,Space.Self);
//公转
//绕某个物体旋转 创建一个空物体，绕空物体旋转
float rotateSpeed = 60;
Transform parent = this.transform.parent;
parent.Rotate(0, rotateSpeed*Time.deltaTime, 0, Space.Self);
```
# day2
- 消息函数
	Awake  初始化，仅调用一次，先于start，即使组件没有被启用
	Start    在首次调用任何 Update 方法之前启用脚本时，在帧上调用 Start。组件脚本上的勾选
	Update
	OnEnable
	OnDisable
- 脚本执行顺序
	多个脚本的执行顺序
	- scripts1.Awake，scripts2.Awake，scripts3.Awake，··
	- scripts1.Start，scripts2.Start，scripts3.Start，···
	- scripts1.Update，scripts2.Update，scripts3.Update，···
	
	Unity 调用每个 GameObject 的 [Awake](https://docs.unity.cn/cn/2022.3/ScriptReference/MonoBehaviour.Awake.html) 的顺序是不确定的。
	优先级
	脚本文件的execution order 添加脚本，指定优先级，值越小优先级越高

- 主控脚本
- 脚本参数
	脚本组件下多了个参数rotate Speed可以设置
```cs
public float rotateSpeed = 1.0f;
```
- 值类型
- 引用类型
- 运行时调试
运行中修改的组件的值不会保存，需要copy component再paste
- 鼠标键盘输入
	待写
-----------------------
- 组件调用  GetComponent
音乐播放  
audio source组件play on awake是否勾选
```cs
if(Input.GetMouseButtonDown(0))
{
	PlayMusic();
}
void PlayMusic()
{
	Debug.Log("** PlayMusic");
	AudioSource audioSource = GetComponent<AudioSource>();
	if (audioSource.isPlaying)
	{
		audioSource.Stop();
	}
	else
	{
		audioSource.Play();
	}
}
```
调用其他脚本组件
```cs
if(Input.GetMouseButtonDown(0))
{
	DoWork();
}
void DoWork()
{
	Debug.Log("** DoWork");
	AudioSource audioSource = GetComponent<AudioSource>();
	if (audioSource.isPlaying)
	{
		audioSource.Stop();
	}
	else
	{
		audioSource.Play();
	}
}
```
- 组件参数
	自身组件参数
	调用其他物体组件参数
	或者直接引用 由检查器去寻找
```cs
//target 其他物体对象
AudioSource audio = target.GetComponent<AudioSource>();
```
- 通过transform组件来获取父级transform组件，再获取GameObject对象
```cs
foreach (Transform child in transform)
	{
		Debug.Log("** child" + child.name);
	}
	
//
this.transform.parent.gameObject
this.transform.Find("bb/cc")//查找bb下的子物体cc
this.transform.Find("/aa")//查找根目录下的子物体aa
```
- 设置新的父级
- 物体的显示和隐藏
```cs
this.transform.SetParent(Transform)
this.transform.SetParent(null)//设为1级节点

Transform child=this.transform.Find("aa");
if(child.gameObject.activeSelf){
	child.gameObject.SetActive(false);
}else{
	child.gameObject.SetActive(true);
}
```
- 定时器 Invoke  InvokeRepeating
```cs
//this.Invoke("DoSomething", 1);
this.InvokeRepeating("DoSomething", 1, 2);

private void DoSomething()
{
	Debug.Log("** dosomething " + Time.time);
}
```
- 定时与线程
```cs
IsInvoking(func)//判断func是否在invoke队列
CancelInvoke(func)//取消func的调用
CancelInvoke()//取消所有调用
```
- 向量
```cs
Vector3.zero
Vector3.up
Vector3.right
Vector3.forward
Vector3.Dot
Vector3.Cross
```
- 向量测距
```cs
magnitude
Vector3.Distance(p2,p1)
```
# day3
- 预制体
动态创建 Instantiate
```cs
GameObject node =  Object.Instantiate(mofangPrefab, cubefolder);
node.transform.position = this.startpoint.position;
//node.transform.localEulerAngles = Vector3.zero;
node.transform.eulerAngles = this.Cylinderrotate.eulerAngles;
```
- 实例销毁 Destroy
```cs
private void selfDestroy()
{
	Debug.Log("** selfDestroy");
	Object.Destroy(this.gameObject);
	//下面还可以做和这个实例相关的事情，实例在这一帧结束后销毁
}
```
- 物理系统
\- 刚体组件 rigidbody
\- 碰撞组件 collider
\- 物理材质