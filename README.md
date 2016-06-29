# Unity AnimationCurvePopupMenu
Unity 曲线编辑扩展菜单功能

# 原因
默认的 AnimationCurve 字段曲线编辑，不支持复制粘贴到另一个 AnimationCurve 字段，亦不支持关键帧的清空。所以扩展 AnimationCurve 的绘制，在右侧添加下拉菜单，以扩展功能。

# 功能
* 复制曲线
* 粘贴曲线
* 清空曲线

# 截图
点击右侧下拉按钮，点击 Copy 复制：
![](https://github.com/akof1314/UnityAnimationCurvePopupMenu/raw/master/Screenshots/screenshot_copy.png)
在另一个曲线，右侧菜单点击 Paste 粘贴：
![](https://github.com/akof1314/UnityAnimationCurvePopupMenu/raw/master/Screenshots/screenshot_copy.png)
清空曲线的关键帧值，点击 Clear 清空：
![](https://github.com/akof1314/UnityAnimationCurvePopupMenu/raw/master/Screenshots/screenshot_clear.png)

# 使用说明
代码放进工程里面，对于默认的脚本，会自动调用扩展的曲线绘制方式：
默认的脚本类似：
```
public class TestCurve : MonoBehaviour
{
    public AnimationCurve curve;
}
```
高级方式，要对默认脚本进行自定义绘制的话，创建 Editor 类。接着，对于属性值，调用 Unity 自带的接口 `EditorGUILayout.PropertyField` 即可。对于非属性值，可通过 AnimationCurveGUI 接口来替换 EditorGUILayout 和 EditorGUI 来绘制曲线：
```
using UnityEngine;
using UnityEditor;

[CustomEditor(typeof(TestCurve))]
public class TestCurveEditor : Editor
{
    private SerializedProperty curveProp;
    private AnimationCurve m_TestAnimCurve = new AnimationCurve();

    void OnEnable()
    {
        curveProp = serializedObject.FindProperty("curve");
    }

    public override void OnInspectorGUI()
    {
        serializedObject.Update();

        EditorGUILayout.PropertyField(curveProp);
        m_TestAnimCurve = AnimationCurveGUI.CurveField("扩展曲线", m_TestAnimCurve);

        serializedObject.ApplyModifiedProperties();
    }
}
```
