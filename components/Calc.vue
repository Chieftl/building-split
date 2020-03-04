<template lang="pug">
  .slidecontainer
    .input-range
      input(type="range" min="100" max="500" v-model="length")
      | {{ length }}
    .input-range
      input(type="range" min="1" max="12" v-model="n")
      | {{ n }}
    .input-range
      input(type="range" min="1" max="2" step="0.1" v-model="k")
      | {{ k }}
    pre {{ len1 }}
    br
    br
    .building(:style="{width:length + 'px'}")
      ul.entrances
        li(v-for="(ent, i) in entrances" :style="{left:enPos(i) + 'px'}")
          | {{ i }}
          span {{ enPos(i) }}
</template>

<script>
let len1 = (length, n, k) => length / (2 + (n-1)*k)
let pos = (i, k, len1) => len1 * (1 + k*i)
let posCoord = (p1, p2, progress) => {
  x = (p2.x - p1.x) * progress + p1.x
  y = (p2.y - p1.y) * progress + p1.y
  return {x,y}
}

export default {
  data() {
    return {
      length: 450,
      n: 5,
      k: 2,
    }
  },
  computed: {
    len1() {
      let length = parseInt(this.length)
      let n = parseInt(this.n)
      let k = parseFloat(this.k)
      return len1(length, n, k)
    },
    entrances() {
      let n = parseInt(this.n)
      return [...Array(n).keys()]
    },
    enPos() {
      return (i) => pos(i, parseFloat(this.k), this.len1)//this.len1 * (1 + parseFloat(this.k)*i) //return (i) => i*40
    }
  }
}
</script>

<style lang="scss">
.building {
  outline: 2px solid blue;
  height: 100px;
}

ul.entrances {
  position: relative;
  list-style: none;
  li {
    position: absolute;
    transform: translate(-50%, -50%);
    //height: 200px;
    //outline: 1px solid red;
    span {
      display: block;
      font-size: .6em;
    }
  }
}
</style>
