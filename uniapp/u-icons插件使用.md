1、先下载插件，将文件夹直接复制到项目
2、在main.js里注册标签<u-icon>
import uIcon from './components/uni-icons/uni-icons.vue‘
Vue.component('u-icon', uIcon)

3、使用方法：
<u-icon type="scan" @click="scanCode"></u-icon>
这样即把scan图标显示出来了。