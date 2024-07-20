<script setup >
    import { defineEmits, defineProps} from "vue"
    let emit = defineEmits(["closewindow"])
    let {ipInfo} = defineProps(["ipInfo"])
    const close = ()=>{
        emit("closewindow", false)
    }
    console.log(ipInfo)
</script>

<template>
    <div class="flex flex-col md:flex-row absolute top-40 w-[150px] md:w-[550px] md:items-center   p-3 bg-white justify-between px-5 rounded-lg">
        <div>
            <h1>Address</h1>
            <p>{{ ipInfo.address }}</p>
        </div>
        <div>
            <h1>location</h1>
            <p>{{ ipInfo.location }}</p>
        </div>
        <div>
            <h1>timezone</h1>
            <p>{{ ipInfo.timezone }}</p>
        </div>
        <div>
            <h1>ISP</h1>
            <p>{{ ipInfo.isp }}</p>
        </div>
        <div><i @click="close" class="fas fa-multiply text-sm font-bold hover:bg-slate-100 rounded-full w-7 pt-1 px-2  h-7" ></i></div>

    </div>
</template>

<style scoped >
    h1{
        font-size: 1.2rem;
        font-weight: bold
    }
    @media (max-width:700px) {
        h1{
            font-size: 15px;
            font-weight: bold;
        }
        p{
            font-size: 12px;
        }
    }
</style>