<template>
  <div class="wrapper">
    <article class="passbox">
      <header>
        <h1>Select options</h1>
      </header>
      <section>
        <div class="slidecontainer">
          <label for="passRange" >length: {{ passwordLength }}</label>
          <input type="range" min="8" max="24" v-model="passwordLength" class="slider" id="passRange">
        </div>
        <div class="options">
          <label class="container" v-for="(option, index) in optiondata" :key="index">
            {{ option.name }}
            <input type="checkbox" v-model="option.status">
            <span class="checkmark"></span>
          </label>
        </div>
      </section>
      <footer>
        <div class="password">{{ generatedPassword }}</div>
        <button class="btn-refresh" @click="refreshPassword = !refreshPassword">refresh</button>
      </footer>
    </article>
  </div>
</template>

<script>
  export default {
    data: function() {
      return {
        passwordLength: 16,
        refreshPassword: false,
        optiondata: [
          {
            name: 'lowercase',
            status: true,
            chars: 'abcdefghjkmnopqrstuvwxyz'
          },
          {
            name: 'uppercase',
            status: true,
            chars: 'ABCDEFGHJKLMNOPQRSTUVWXYZ'
          },
          {
            name: 'numbers',
            status: true,
            chars: '0123456789'
          },
          {
            name: 'specialchars',
            status: false,
            chars: '!$%&?+*#-/'
          }
        ]
      }
    },
    computed: {
      charset() {
        return [...this.optiondata]
                .map(element => {
                  if(element.status === true)
                    return element.chars;
                }).join('');
      },
      generatedPassword() {
        this.refreshPassword;

        return [...window.crypto.getRandomValues(new Uint32Array(this.passwordLength))]
                .map(value => this.charset[value % this.charset.length])
                .join('');
      }
    }
  }
</script>

<style lang="scss" scoped>
  .wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100vh;
  }

  .passbox {
    width: 550px;
    border-radius: 10px;
    padding: 0 0 10px;
    background-color: #ecf0f1;
    text-align: center;
    overflow: hidden;
  }

  header {
    padding: 10px 0;
    margin-bottom: 10px;
    background-color: #e74c3c;

    h1 {
      margin: 0;
      color: #fff;
    }
  }

  section {
    .slidercontainer {
      width: 100%;
    }

    .slider {
      appearance: none;
      width: calc(100% - 20px);
      height: 8px;
      margin-bottom: 20px;
      background-color: #95a5a6;
      outline: none;
      opacity: 0.7;
      transition:  opacity .2s;

      &:hover {
        opacity: 1;
      }

      &::-webkit-slider-thumb {
        appearance: none;
        width: 30px;
        height: 30px;
        border: 2px solid #e74c3c;
        border-radius: 50%;
        background-color: #f1c40f;
        cursor: pointer;
      }
    }

    .options {
      display: flex;
      justify-content: space-around;
      align-items: center;
      flex-wrap: wrap;
      padding: 20px;

      .container {
        display: block;
        position: relative;
        width: 160px;
        padding-left: 35px;
        margin-bottom: 12px;
        font-size: 1.4rem;
        user-select: none;
        cursor: pointer;

        &:hover input ~ .checkmark {
          background-color: #f1c40f;
        }
      }

      .container input {
        position: absolute;
        cursor: pointer;
        opacity: 0;
        height: 0;
        width: 0;

        &:checked ~ .checkmark {
          background-color: #e74c3c;

          &:after {
            display: block;
          }
        }
      }

      .checkmark {
        position: absolute;
        top: 0;
        left: 0;
        height: 25px;
        width:25px;
        background-color: #95a5a6;

        &:after {
          content: "";
          position: absolute;
          display: none;
          left: 9px;
          top:5px;
          width: 5px;
          height: 10px;
          border: solid #fff;
          border-width: 0 3px 3px 0;
          transform: rotate(45deg);
        }
      }
    }
  }

  footer {
    .password {
      font-size: 2rem;
      overflow-wrap: break-word;
      padding: 5px 0;
      margin-bottom: 15px;
      border: solid #ccc;
      border-width: 2px 0;
    }
    
    .btn-refresh {
      font-size: 2rem;
      border: none;
      border-radius: 5px;
      background-color: #e74c3c;
      color: #fff;
      padding: 5px 10px;
      outline:none;
      cursor: pointer;
    }
  }
</style>
