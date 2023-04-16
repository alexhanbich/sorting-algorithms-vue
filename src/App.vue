<script>
export default {
  data() {
    return {
      arr: [],
      size: 50,
      speed: 10,
    }
  },
  mounted() {
    this.generateArray();
  },
  methods: {
    generateArray() {
      this.arr.splice(0);
      for (let i = 0; i < this.size; i++) {
        this.arr.push(i + 1)
      }
      this.shuffleArray()      
    },
    shuffleArray() {
      for (let i = this.size - 1; i > 0; i--) {
          let j = Math.floor(Math.random() * (i + 1));
          [this.arr[i], this.arr[j]] = [this.arr[j], this.arr[i]];
      }
    },
    sleep() {
      return new Promise((resolve) => setTimeout(resolve, this.speed));
    },
    async bubbleSort(){
      let len = this.arr.length
      for (let i = 0; i < len; i++) {
        for (let j = 0; j < (len - i - 1); j++) {
          if (this.arr[j] > this.arr[j+1]) {
            [this.arr[j], this.arr[j+1]] = [this.arr[j+1], this.arr[j]];
            await this.sleep()
          }
        }
      }
    },
    async insertionSort() { 
        for (let i = 1; i < this.arr.length; i++) { 
            let j = i
            while (j > 0 && this.arr[j-1] > this.arr[j]) { 
              [this.arr[j], this.arr[j-1]] = [this.arr[j-1], this.arr[j]];
              await this.sleep()
              j = j - 1; 
            } 
        } 
    },
    async quickSortRecursive(left, right) {
        let pivot = this.arr[Math.floor((left + right) / 2)];
        let i = left;
        let j = right;
        while (i <= j) {
            while (this.arr[i] < pivot) {
                i++;
            }
            while (this.arr[j] > pivot) {
                j--;
            }
            if (i <= j) {
                [this.arr[i], this.arr[j]] = [this.arr[j], this.arr[i]];
                await this.sleep()
                i++;
                j--;
            }
        }
        let index = i
        if (left < index - 1) {
            this.quickSortRecursive(left, index - 1);
        }
        if (index < right) {
            this.quickSortRecursive(index, right);
        }
        return;
      },
      quickSort() {
        let left = 0;
        let right = this.arr.length - 1;
        this.quickSortRecursive(left, right)
        
      },
  }
}
</script>

<template>
  <h1>Sorting Algorithms</h1>
  <div class="container">
    <div className="bar" v-for="(val, i) in arr" :key="i" :style="{height: val*3+'px'}">
    </div>
  </div>
  <div class="buttons">
    <button @click="shuffleArray()">Shuffle</button>
    <button @click="bubbleSort()">Bubble Sort</button>
    <button @click="insertionSort()">Insertion Sort</button>
    <button @click="quickSort()">Quick Sort</button>
  </div>
  <div class="inputs">
    <div class="input-container">
      <p>Size: </p>
      <input v-model="size" type="range" min="10" max="100" class="slider" />
      <input v-model="size" type="number" />
      <button @click="generateArray()">Generate Array</button>
    </div>
    <div class="input-container">
      <p>Miliseconds Between Animation: </p>
      <input v-model="speed" type="range" min="10" max="200" class="slider" />
      <input v-model="speed" type="number" />
    </div>
  </div>
</template>

<style>
html * {
  font-family: "Lucida Console", "Courier New", monospace;
}

h1 {
  text-align: center;
}

.container {
  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.bar {
  width: 8px;
  background-color: black;
  display: inline-block;
  margin: 0 1px;
}

.buttons {
  display: flex;
  justify-content: space-around;
  margin: 30px 0;
}

.input-container {
  display: flex;
  align-items: center;
  margin: 0 30px;
}

.inputs {
  display: flex;
  justify-content: space-around;
  margin: 30px 30px;
}
</style>
