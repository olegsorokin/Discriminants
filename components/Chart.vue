<template>
  <div class="chart">
    <div class="chart-box">
      <svg class="chart-image" width="500" height="500" viewBox="0 0 1 500">
        <polyline fill="none" stroke="black" points="0,-250 0,500"></polyline>
        <polyline fill="none" stroke="black" points="-250,250 250,250"></polyline>
        <line v-for="item of lines()"
              :x1="item"
              :y1="centerPoint - 3"
              :x2="item"
              :y2="centerPoint + 3"
              class="chart-separator"
              stroke="black"/>
        <line v-for="item of lines()"
              x1="-3"
              :y1="item + centerPoint"
              x2="3"
              :y2="item + centerPoint"
              class="chart-separator"
              stroke="black"/>

        <polyline fill="none" stroke="red" stroke-linejoin="round" :points="graphPath"/>
      </svg>

      <div class="input-group chart-scale">
        <div class="input-group-prepend">
          <span class="input-group-text">M</span>
        </div>
        <input v-model="scale"
               :class="{ 'is-invalid': $v.scale.$error }"
               @blur="$v.scale.$touch()"
               type="number"
               class="form-control"
               aria-label="scale"
               min="0.1"
               :max="maxScale"
               step="0.1"
               required>
        <div class="invalid-feedback">
          Значение поля не является числом
        </div>
      </div>

      <label class="chart-scale-range">
        <input v-model="scale" min="0.1" step="0.1" :max="maxScale" type="range" class="form-control-range">
      </label>
    </div>
  </div>
</template>

<script>
  import { required, between } from 'vuelidate/lib/validators';

  export default {
    name: 'Chart',
    props: {
      valueA: {
        type: Number,
        default: 0
      },
      valueB: {
        type: Number,
        default: 0
      },
      valueC: {
        type: Number,
        default: 0
      },
    },
    data() {
      return {
        scale: 1,
        maxScale: 5,
        centerPoint: 250,
        coordinatesStep: 25
      };
    },
    computed: {
      graphPath() {
        const step = 0.2;
        let y = 0;
        let line = '';

        for (let i = -1 * this.centerPoint; i < this.centerPoint; i += step) {
          y = (this.valueA * i * i + this.valueB * i + this.valueC) * -1 + this.centerPoint * this.scale;
          line += (i / this.scale) + ',' + (y / this.scale) + ' ';
        }

        return line;
      }
    },
    validations: {
      scale: {
        required,
        between: between(0.1, 5)
      }
    },
    methods: {
      lines() {
        let coordinates = [];
        for (let i = -1 * this.centerPoint * 9; i < this.centerPoint * 9; i = i + this.coordinatesStep) {
          coordinates.push(i / this.scale);
        }

        return coordinates;
      }
  }
  }
</script>
