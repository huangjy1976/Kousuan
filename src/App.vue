<template>
  <main>
    <div style="margin-bottom:20px;" class="header">
      <a href="#" class="active" @click="setMax(10, 1, 10, 2, $event)">10以内</a> &nbsp; &nbsp; <a href="#" @click="setMax(20, 10, 10, 2, $event)">20以内（第二个数10以内）</a> &nbsp; &nbsp; <a href="#" @click="setMax(20, 10, 20, 2, $event)">20以内</a> &nbsp; &nbsp; <a href="#" @click="setMax(20, 1, 20, 3, $event)">20以内（3个数）</a> &nbsp; &nbsp; <a href="#" @click="setMax(100, 20, 10, 1, $event)">100以内(第二数10以内)</a> &nbsp; &nbsp; <a href="#" @click="setMax(100, 20, 100, 2, $event)">100以内</a> &nbsp; &nbsp; 
       生成 <input type="text" v-model="numbers" @keyup.enter="generate()" style="width: 30px;"> 道题  &nbsp; &nbsp; <input type="button" value="生成" @click="generate()" style="width: 50px; height: 30px; padding: 2px;"> &nbsp; &nbsp; <a href="#" @click="hideHeader">隐藏该区域（方便打印使用）</a>
    </div>
    <div style="height: 200px;">
      <div v-for="test in tests" style="float: left; font-size: 16px; padding:5px; padding-top: 10px;" :style="type == 3 ? 'width: 170px; ':'width: 120px; '">{{test}}</div>
    </div>
  </main>
</template>

<style>
a {text-decoration: none}
.active {color:blue; text-decoration: underline;}
</style>

<script>
export default {
  name: 'main',
  data() {
    return {
      hide:false,
      max: 20,
      min: 10,
      numbers: 40,
      tests: [],
      max2: 10,
      type: 2,
    }
  },
  methods: {
    hideHeader() {
      $(".header").addClass("hide-header");
    },
    /** set parameters:
     * max: max number, min: min number, max2: second max number
     * type: 2: 2 numbers; 3: 3 numbers
     */
    setMax(max, min, max2, type, event) {
      this.max = max;
      this.max2 = max2;
      this.min = min;
      this.type = type;
      this.generate();

      $("a.active").removeClass("active");
      $(event.target).addClass("active");
    },
    shuffle(a) {
      for (let i = a.length; i; i--) {
        let j = Math.floor(Math.random() * i);
        [a[i - 1], a[j]] = [a[j], a[i - 1]];
      }
    },
    generate() {
      let all = [];
      this.tests = [];
      for (let i = this.min; i < this.max; i++) {
        for (let n = 1; n < this.max2 && n < this.max-i; n++) {
          all.push(i + " + " + n + " = ");
          if (i != 1 && n != 1 && this.max < 20)
            all.push(i + " + " + n + " = ");
        }
      }

      for (let i = this.min+1; i < this.max + 1; i++) {
        for (let n = 1; n < this.max2 && n < i - 1; n++) {
          all.push(i + " - " + n + " = ");
          if (i != 1 && n != 1 && this.max < 20)
            all.push(i + " - " + n + " = ");
        }
      }
      this.shuffle(all);
      for (let i = 0; i < this.numbers; i++) {
        // get the third number
        if (this.type == 3) {
          let t = all[i].replace(" = ", "");
          let s = eval(t);
          s = this.getThird(s);
          this.tests.push(t + s + " = ");
        } else {
          this.tests.push(all[i]);
        }
      }

    },
    // get third number
    getThird(number) {
      let temp = [];
      // plus
      for (let i = 1; i < this.max - number; i++)
        temp.push(" + " + i);

      // minus
      for (let i = 1; i < number; i++)
        temp.push(" - " + i);

      // get random 1
      this.shuffle(temp);
      return temp[0];
    }
  },
  created: function() {
    this.generate();
    return;
    for (let i = 0; i < 125; i++) {
      let r = this.getRandom(0,10);
      if (r > 5) {
        let num1 = this.getRandom(3, 10);
        let num2 = this.getRandom(2, num1-1);
        this.tests.push("" + num1 + " - " + num2 + " = ");
      } else {
        let num1 = this.getRandom(2, 8);
        let num2 = 10 - num1;
        let num3 = this.getRandom(2, num2);
        this.tests.push("" + num1 + " + " + num3 + " = ");
      }
    }
  }
}
</script>
