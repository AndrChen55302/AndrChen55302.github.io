---
title: vue使用dialog
date: 2021-09-03
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630743484000.png
---
## 前言
初学vue时做demo练习时  喜欢跳转另一个vue页面进行操作，这次来点不一样的


{% note danger modern %}
当前 前提你得引入elementui
{% endnote %}

### 使用
```vue
<el-button type="text" @click="centerDialogVisible = true">点击打开 Dialog</el-button> //一个点击事件 当点击的时候 打开dialong
	<el-dialog title="提示" :visible.sync="centerDialogVisible" width="50%" center>
		<!-- title:是 头部  中间的div是body  #footer是底部 -->
		<div>
			<span>需要注意的是内容是默认不居中的</span>
			<el-form :label-position="labelPosition" :model="cityOne" :disabled="true" label-width="80px">
				<el-form-item label="城市区号">
					<el-input v-model="cityOne.citycode"></el-input>
				</el-form-item>
				<el-form-item label="城市编号">
					<el-input v-model="cityOne.id"></el-input>
				</el-form-item>
			</el-form>
		</div>
		<span slot="footer" class="dialog-footer">
			<el-button @click="centerDialogVisible = false">取 消</el-button>
			<el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
		</span>
	</el-dialog>
	export default {
		data() {
                   return{
                     cityOne:[], //dialong双向绑定的对象
                     centerDialogVisible: false, //控制dialong的显隐  默认不显示
                     labelPosition: 'right', //控制对齐方式  前提需要一个宽度
               }
            }
        }

```