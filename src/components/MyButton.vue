<template>
    <div>
        hello
        <slot></slot>  
        <!-- slot 是这个组件标签的出口组件 -->
    </div>
</template>
<script>
export default {
    name:"MyButton"
}
</script>