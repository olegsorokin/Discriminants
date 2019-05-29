<template>
  <section class="container">
    <h1 class="page-title">Рассчёт квадратного трёхчлена</h1>

    <div class="row">
      <div class="col-md-6 mb-3">
        <chart :value-a="Number(valueA)" :value-b="Number(valueB)" :value-c="Number(valueC)"/>
      </div>

      <div class="col-md-6">
        <p>f(x) = ax<sup>2</sup> + bx + c</p>
        <form @submit.prevent="checkForm">
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text">a</span>
            </div>
            <input v-model.number="valueA"
                   :class="{ 'is-invalid': $v.valueA.$error }"
                   @blur="$v.valueA.$touch()"
                   type="number"
                   class="form-control"
                   placeholder="Введите значение a"
                   aria-label="valueA"
                   required>
            <div class="invalid-feedback">
              Значение поля не является числом
            </div>
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text">b</span>
            </div>
            <input v-model.number="valueB"
                   :class="{ 'is-invalid': $v.valueB.$error }"
                   @blur="$v.valueB.$touch()"
                   type="number"
                   class="form-control"
                   placeholder="Введите значение b"
                   aria-label="valueA"
                   required>
            <div class="invalid-feedback">
              Значение поля не является числом
            </div>
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text">c</span>
            </div>
            <input v-model.number="valueC"
                   :class="{ 'is-invalid': $v.valueC.$error }"
                   @blur="$v.valueC.$touch()"
                   type="number"
                   class="form-control"
                   placeholder="Введите значение c"
                   aria-label="valueA"
                   required>
            <div class="invalid-feedback">
              Значение поля не является числом
            </div>
          </div>

          <button :disabled="$v.$invalid" type="submit" class="btn btn-dark" @click="discriminant()">Рассчитать</button>

          <div class="result" v-if="result">
            <strong>Ответ:</strong>
            {{ result }}
          </div>
        </form>
      </div>
    </div>
  </section>
</template>

<script>
  import { required } from 'vuelidate/lib/validators';
  import Chart from '../components/Chart';

  export default {
    components: {
      Chart
    },
    data() {
      return {
        valueA: null,
        valueB: null,
        valueC: null,
        result: ''
      };
    },
    methods: {
      discriminant() {
        return this.valueB * this.valueB - 4 * this.valueA * this.valueC;
      },
      calculateX(a, b, c) {
        return b * b - 4 * a * c;
      },
      calculateX1(a, b, c, d) {
        return (-b + Math.sqrt(d)) / (2 * a);
      },
      calculateX2(a, b, c, d) {
        return (-b - Math.sqrt(d)) / (2 * a);
      },
      checkForm() {
        this.$v.$touch();

        if (this.$v.$error) {
          return;
        }

        let a = Number(this.valueA);
        let b = Number(this.valueB);
        let c = Number(this.valueC);

        if (a === 0 && b === 0) {
          this.result = 'x = 0';
          return;
        } else if (a === 0 && b !== 0) {
          this.result = 'x = ' + (-1 * c / b);
          return;
        }

        if (this.discriminant() < 0) {
          this.result = 'D < 0. Нет вещественных корней.';
        } else if (this.discriminant() === 0) {
          this.result = 'x = ' + this.calculateX(a, b, c);
        } else {
          let x1 = this.calculateX1(a, b, c, this.discriminant());
          let x2 = this.calculateX2(a, b, c, this.discriminant());
          this.result = 'x1 = ' + x1 + ', x2 = ' + x2;
        }
      }
    },
    validations: {
      valueA: { required },
      valueB: { required },
      valueC: { required }
    }
  }
</script>
