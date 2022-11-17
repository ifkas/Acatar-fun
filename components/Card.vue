<template>
  <div class="container mx-auto border rounded-2xl pt-24">
    <h1 class="text-center text-2xl mb-4 font-medium">Fun card ID generator</h1>
    <div
      class="
        bg-indigo-500
        px-8
        py-12
        flex
        place-content-stretch
        flex-wrap
        justify-around
        items-start
        background-gradient
        border
        rounded-2xl
        border-gray-200
        shadow-2xl
      "
      v-bind:class="[isActive ? styleOne : styleTwo]"
    >
      <div class="left w-double-px">
        <div class="p-10 glass-card">
          <div v-if="$fetchState.pending">
            <Loader />
          </div>
          <div v-else-if="$fetchState.error">
            <img :src="avatarUrl" />
            <div class="mt-5">
              <h1 class="text-2xl">Ivo Culic</h1>
              <h3>Web Developer</h3>
            </div>
          </div>
          <div v-else>
            <div class="second-morph">
              <img :src="avatarUrl" />
            </div>
            <div class="mt-5">
              <h1 class="text-2xl w-px">{{ this.anotherName.ime }}</h1>
              <h3>{{ this.anotherName.rabota }}</h3>
            </div>
          </div>
        </div>
        <div class="mb-8 flex flex-col">
          <button class="mt-8 py-2 px-1 glass-card font-medium" @click="$fetch">
            Generate
          </button>
        </div>
      </div>
      <div class="right p-10 glass-card">
        <div class="flex flex-col">
          <div class="mb-8 flex flex-col">
            <h3 class="text-xl mb-6">Let's have fun?</h3>
            <label
              class="block text-gray-700 text-sm font-bold mb-2"
              for="name"
            >
              Write your name
            </label>
            <input
              @keyup.enter="$fetch"
              v-model="anotherName.ime"
              class="
                appearance-none
                border
                rounded
                w-full
                py-2
                px-3
                text-gray-700
                leading-tight
                focus:outline-none focus:shadow-outline
              "
              id="name"
              type="text"
              placeholder="Ivo Culic"
            />
          </div>
          <div class="mb-8 flex flex-col">
            <label for="toggleCorona" class="flex items-center cursor-pointer">
              <!-- toggle -->
              <div class="relative">
                <!-- input -->
                <input
                  @change="$fetch"
                  v-model="coronaTime"
                  type="checkbox"
                  id="toggleCorona"
                  class="sr-only"
                />
                <!-- line -->
                <div
                  class="block bg-transparent-small w-14 h-8 rounded-full"
                ></div>
                <!-- dot -->
                <div
                  class="
                    dot
                    absolute
                    left-1
                    top-1
                    bg-white
                    w-6
                    h-6
                    rounded-full
                    transition
                  "
                ></div>
              </div>
              <!-- label -->
              <div class="ml-3 text-gray-700 text-sm font-bold">
                Corona time?
              </div>
            </label>
          </div>
          <div class="mb-8 flex flex-col">
            <label for="toggleSmile" class="flex items-center cursor-pointer">
              <!-- toggle -->
              <div class="relative">
                <!-- input -->
                <input
                  @change="$fetch"
                  v-model="superSmile"
                  type="checkbox"
                  id="toggleSmile"
                  class="sr-only"
                />
                <!-- line -->
                <div
                  class="block bg-transparent-small w-14 h-8 rounded-full"
                ></div>
                <!-- dot -->
                <div
                  class="
                    dot
                    absolute
                    left-1
                    top-1
                    bg-white
                    w-6
                    h-6
                    rounded-full
                    transition
                  "
                ></div>
              </div>
              <!-- label -->
              <div class="ml-3 text-gray-700 text-sm font-bold">
                Super smile? (Anti-masker)
              </div>
            </label>
          </div>
          <div class="mb-8 flex flex-col">
            <label for="toggleSelfie" class="flex items-center cursor-pointer">
              <!-- toggle -->
              <div class="relative">
                <!-- input -->
                <input
                  @change="$fetch"
                  v-model="badSelfie"
                  type="checkbox"
                  id="toggleSelfie"
                  class="sr-only"
                />
                <!-- line -->
                <div
                  class="block bg-transparent-small w-14 h-8 rounded-full"
                ></div>
                <!-- dot -->
                <div
                  class="
                    dot
                    absolute
                    left-1
                    top-1
                    bg-white
                    w-6
                    h-6
                    rounded-full
                    transition
                  "
                ></div>
              </div>
              <!-- label -->
              <div class="ml-3 text-gray-700 text-sm font-bold">
                Bad selfie?
              </div>
            </label>
          </div>
          <div class="mb-8 flex flex-col">
            <button
              class="p-2 font-medium"
              v-bind:class="[isActive ? styleTwo : styleOne]"
              @click="colorBg"
            >
              {{ isActive ? 'Dark Background' : 'Light Background' }}
            </button>
          </div>
        </div>
      </div>
      <!-- End of right -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      avatarName: '',
      avatarUrl:
        'https://avatars.dicebear.com/api/open-peeps/io.svg?size=200&head=long&skinColor=variant02',
      anotherName: {
        ime: '',
      },
      isActive: false,
      styleOne: 'styleOne',
      styleTwo: 'styleTwo',
      coronaTime: false,
      badSelfie: false,
      superSmile: false,
    };
  },
  async fetch() {
    //this.avatarName = (Math.random() + 1).toString(36).substring(7);
    if (this.anotherName.ime === '') {
      const second = 'https://retoolapi.dev/0zflLl/data-card-generator';
      this.anotherName = await fetch(second).then((res) =>
        res
          .json()
          .then(
            (data) =>
              (this.anotherName = data[Math.floor(Math.random() * data.length)])
          )
      );
    } else {
      //this.coronaTime = false;
    }

    let Mask;
    if (this.coronaTime != '') {
      Mask = '&mask[]=medicalMask&maskProbability=100';
    } else {
      Mask = '';
    }

    let Selfie;
    if (this.badSelfie != '') {
      Selfie = '&scale=200';
    } else {
      Selfie = '';
    }

    let Smile;
    if (this.superSmile != '') {
      Smile = '&face=smileBig';
    } else {
      Smile = '';
    }

    //console.log(this.anotherName.ime);

    let imageUrl =
      'https://avatars.dicebear.com/api/open-peeps/' +
      this.anotherName.ime +
      '.svg?size=200' +
      Mask +
      Selfie +
      Smile;
    ('');

    await fetch(imageUrl)
      .then((response) => response.blob())
      .then((imageBlob) => {
        // Then create a local URL for that image and print it
        this.avatarUrl = URL.createObjectURL(imageBlob);
        //console.log(imageObjectURL);
      });
  },
  fetchDelay: 1000,

  methods: {
    colorBg() {
      this.isActive = !this.isActive;
    },
  },
};
</script>
<style>
.container {
  max-width: 720px;
}
.glass-card {
  background: rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(2px);
  -webkit-backdrop-filter: blur(2px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.18);
}
.second-morph {
  background: rgba(237, 237, 237, 0.5);
  /* box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37); */
  backdrop-filter: blur(7px);
  -webkit-backdrop-filter: blur(7px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.18);
}
.styleOne {
  background: linear-gradient(90deg, #a4baf5, #c8e74d, #f5a2a1);
}
.styleTwo {
  background: linear-gradient(90deg, #03256c, #1768ac, #e5d549);
}
button.styleTwo {
  color: #fff;
}
.background-gradient {
  background-size: 300% 300%;
  animation: gradient 6s alternate infinite;
}
@keyframes gradient {
  0% {
    background-position: 0%;
  }
  100% {
    background-position: 100%;
  }
}
/* Toggles */
.bg-transparent-small {
  background: rgba(255, 255, 255, 0.2);
}
input:checked ~ .dot {
  transform: translateX(100%);
  background-color: #ccc;
}
.w-px {
  width: 200px;
  word-break: break-all;
}
.w-double-px {
  width: 290px;
}
</style>
