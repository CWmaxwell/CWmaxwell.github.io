#react-native安卓开发遇到的坑

##1、StackNavigator导航栏标题对象无法居中
解决方法：navigationOptions对象下的属性headerTitleStyle设置alignSelf:'center'，
![Alt text](./set_headerTitleStyle.png)
一般即可解决问题，如果还未解决，需要修改your_project_name/node_modules/react-navigation/src/views/Header/Header.js文件中的title样式属性
![Alt text](./set_title_style_justifyContent.png)



##2、ScrollView设置水平翻转时出现RDS
错误原因如下图所示
![Alt text](./wrong_place_ScrollView.png)
临时解决方法：
将react-native版本回退到“0.49.5”版本（在package.json里修改，修改后在project目录下执行npm install命令），0.50版本上的的Android6.0、7.0几乎都会遇到此问题（即便时最新的稳定0.54.0版本也没修复此问题）
更新：将react-native版本升级到0.54.2或更高版本可解决该问题

##3、react-native-swiper组件在安卓机上设置高度无效
解决方法： 在Swiper组件外层嵌套一个View，设置View的高度即可

##4、安卓下网络图片加载失败
检查以下几点：
1.Android Manifest文件设置网络连接许可
 android:name="android.permission.INTERNET"
 2.请求网络图片时一定要指定图片的大小（本地图片不用）
 3.关键字是 uri 而不是 url ！！！！

##5、安卓下使用react-native-swiper设置垂直滑动无效

horizontal={false} 没有效果，只是分页器变到了右边，但切换滑动时依旧是水平效果，而不是垂直效果。这是官方的问题。
临时解决方法步骤：
1.在 projectname/node_modules/react-native-swiper/package.json
文件里将“version"一行的版本改成 1.5.14；并在dependencies里添加
![Alt text](./react_native_swiper_packagejson.png)
2.在 projectname/node_modules/react-native-swiper/src/index.js
添加
import { StyleSheet } from 'react-native'
import VertViewPager from 'react-native-vertical-view-pager' ；
将641行开始的return语句块替换成下面的
![Alt text](./react_native_swiper_src_index.png)
3，还是在 projectname/node_modules/react-native-swiper/src/index.js里，将467行的
if (Platform.OS !== 'ios')   替换成
if (Platform.OS !== 'ios' && this.props.horizontal === true)

##6、安卓使用react-native-swiper有时会出现RDS，报错提示是property 'x' of undefined
 报错位置在react-native-swiper/src/index.js文件的400行左右，
解决方法有两种：
第一种（优解）：将该附近代码替换成以下
![Alt text](./react_native_swiper_src_index2.png)
第二种：使用react-native-swiper时设置autoPlay属性为{false}，生成离线包时再根据需求是否换成{true}


 