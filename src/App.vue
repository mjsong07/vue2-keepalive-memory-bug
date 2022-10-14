<template>
    <div>
        <div>keep-alive includeList:{{indexNameList}}</div>
        <button @click="routerAdd()">add</button> <button @click="routerDel()">delete</button> <span>usedJSHeapSize<b id="usedJSHeapSize"></b></span>
        <div class="keepaliveBox">
            <keep-alive :include="indexNameList">
                <router-view />
            </keep-alive>
        </div>
        <div class="barBox">
            <div class="box" v-for="(index) in indexList" :key="index">
                <span @click="routerClick(index)">a{{index}}</span>
                <button @click="routerDel(index)">x</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            indexList: [],
            usedJSHeapSize: ''
        }
    },
    mounted() {
        const usedJSHeapSize = document.getElementById("usedJSHeapSize")
        window.setInterval(() => {
            usedJSHeapSize.innerHTML = (performance.memory.usedJSHeapSize / 1000 / 1000).toFixed(2) + "mb"
        }, 1000)
    },
    computed: {
        indexNameList() {
            const res = ['index']//
            this.indexList.forEach(index => {
                res.push(`a${index}`)
            }) 
            return res
        }
    },
    methods: {
        routerAdd() {
            let index = 0
            this.indexList.length > 0 && (index = Math.max(...this.indexList)) 
            index++ 
            this.indexList.push(index)
            this.$router.$append(index)
            this.$router.$push(index)
        },
        routerDel(index) { 
            if (this.indexList.length == 0) return
            if(!index) {
                index = Math.max(...this.indexList)
            }
            if (this.$route.path !== '/index') { 
                this.$router.push('/index') 
            }
            let delIndex = this.indexList.findIndex((item) => item == index)
            this.$delete(this.indexList, delIndex)
        },
        routerClick(index) {
            this.$router.$push(index)
        },
    }
};
</script>

<style scoped>
.keepaliveBox {
    border: 1px solid red;
    padding: 3px;
}

.barBox {
    display: flex;
    flex-wrap: wrap;
}

.box {
    margin: 2px;
    min-width: 70px;
}

.box>span {
    padding: 0 2px;
    background: black;
    color: #fff;
}
</style>