---
title: 'vue实现多选和单选'
date : '2016-05-06'
commentss : true
categories : 'VUE随笔'
---

近期写vue项目,遇到了很多需要select的页面,select坑实在多的填补上了,索性直接select直接vue 实现了,话不多说上代码
```vue
<template>
    <div class="imgManagement w100">
        <ul class="flex-start-warp w100" >
            <li class="relative">
                <input type="file" @change="fileChange($event,imgList)" accept="image/*" multiple> 
                <img id="mange" src="../../assets/images/wechat/workerCars/选择案例图片@2x.png" alt="">
            </li>
            <li class="flex-ajc relative" v-for="(item,index) in imgList" :key="index">
                <div>
                    <img @click="deleteImg(index)" src="../../assets/images/wechat/workerCars/关闭@2x.png" alt="">
                </div>
                <img id="imgList" :src="item" alt="">
            </li>
        </ul>
        <div class="footer w100 fz17" @click="uploadFile(name,imgList,callback)">
            <p>确定</p>
        </div>
    </div>
</template>
<script>
import aliOss from '../../tools/aliUpload'
export default {
    name : 'imgManagement',
    data () {
        return {
            imgList : [], //图片缓存
            name : 'h5test/8609'
        }
    },
    created () {

    },
    mounted () {
        let el = document.querySelector('li'),
            elLi = document.querySelectorAll('li'),
            inputs = document.querySelector('input'),
            width = Math.round(el.offsetWidth);
           elLi.forEach(element => {
               element.style.height = width + 'px';
           });
           inputs.style.width = width - 10 + 'px';
           inputs.style.height = width - 10 + 'px';

    },
    updated () {
        let img = document.querySelectorAll('#imgList'),
             el = document.querySelector('#mange'),
            width = Math.round(el.offsetWidth);
            img.forEach(el => {
               el.style.height = width + 'px';
               el.style.width = width + 'px';
           })
    },
    methods : {
        //点击按钮删除图片
        deleteImg ($index) {
            console.log($index)
            this.imgList.splice($index,1); 
        },

        //点击添加图片
        fileChange (el,imgList) {
            aliOss.fileChange(el,imgList).then(res => {
                console.log(res);
            });
        },
        callback() {
          this.$router.push({path : '/choose'})
        },
        //点击上传图片
        uploadFile (name,list,callback) {
           
                aliOss.uploadFile(name,list,callback).then(res=> {
                console.log(res)
            })
    
        }
    }
    
}
</script>
<style scoped>
    .footer {
        position: fixed;
        bottom: 0;
        left: 0;
    }
    .footer p{
        line-height: 5rem;
        width: 100%;
        background-color: rgba(255, 209, 2, 1);
        text-align: center;

    }
</style>

```
声明两个空数组list1  list2 ,一个缓存所有list1 列表index 另一个缓存选中list2 index 每次选中时 向list2 中push list1 的, push之前includes  要推入的index  是否存在与list2,以此完成页面的单选多选是删除等功能.
