<template>
    <div class="layout">
    
    <div class="container">
        <div class="row">
            <div class="col-sm-1">
            </div>
             <div class="col-sm-12">
                 <slot></slot>
            </div>
        </div>
    </div>
    </div>
</template>
<script>
export default {
    name: 'Layout'
  
}
</script>
<style scoped>
.container{
    margin-top: 60px;
}
</style>