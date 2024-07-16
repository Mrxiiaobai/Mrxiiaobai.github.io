---
categories: vue
tags: vue组件的v-model
title: vue组件的v-model
---

```javascript
// Child
<template>
</template>
<script>
export default {
    props: {
         msg: { type: string }   
    }，
    model: {
        prop: 'msg',
        event: 'input'
    },
    data: {
        value: null    
    },
    computed: {
        defaultValue:{
            set(val){
                this.val = value
                this.$emit('input', val)          
            },
            get(){
                return this.value            
            }        
        }
    },
    created(){
        this.defaultValue = msg    
    },
    methods: {
        onClick(){
            // 这里便会触发emit input方法
            this.defaultValue = 'sdf'
        }    
    }
}
</script>

// Parent.vue 使用
<Child v-model='' />
```

