<script setup lang="ts">
import { ref } from 'vue'


class Time {
    hour: number
    min: number
    constructor(h:number,m:number) {
        this.hour = h
        this.min = m
    }
    add(n:number) {
        this.min += n
        if (this.min >= 60) {
            this.hour += this.min/60|0
            this.min %= 60
        }
    }
    str():string {
        return `${this.hour}:${('00'+this.min).slice(-2)}`
    }
}

const output = ref('')
const time = ref('')
const interval = ref(0)

function process() {
    const s = convertToTime(time.value)
    output.value = makeSchedule(interval.value,s,Array(7).fill('test'))
}

function convertToTime(t:string): Time {
    const [h,m] = t.split(':')
    return new Time(Number(h),Number(m))
}


function makeSchedule(i:number, t:Time, title:Array<string>) {
    let s = ''
    for (const n of title) {
        let e:string = `${t.str()}~`
        t.add(i)
        e += `${t.str()} / ${n}\n`
        s += e
    }
    return s
}
</script>
<template>
    <div class="container">
        <textarea name="output" id="output" cols="30" rows="10" v-model="output"></textarea>
        <div class="args">
            <input type="time" v-model="time" />
            <input type="number" v-model="interval"/>
        </div>
        <button @click="process()">push me</button>
    </div>
</template>
<style scoped>
.container {
    display: flex;
    flex-direction: column;
}
.args {
    display: flex;
    align-items: center;
    margin: 1rem;
    justify-content: center;
    gap: 1rem;
}
</style>