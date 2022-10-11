<template>
    <div>VueComponent<input v-model="myname"/> </div>
</template>
<script>
export default {
    name:'A',
    data(){
        return {
            a:new Array(20000000).fill(1),//大概80mb
            myname:""
        }
    },
    mounted(){  
        this.myname = this.$route.query.name
    }
}
</script>