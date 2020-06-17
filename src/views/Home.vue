<template>
  <div class="home">
    <h1>Metadata Reader</h1>
    <input type="file" @change="updatedInput">
    <img id="img1" style="width:480px" src="../assets/IMG_5567.jpeg" alt="">


    <br><br>
    <div id="output"></div>
    <b-list-group>
      <b-list-group-item v-for="(value,key) in info" :key="key">{{key}}: {{value}}</b-list-group-item>
    </b-list-group>
  </div>
</template>

<script>
  // @ is an alias to /src
  import EXIF from 'exif-js'
  import exif2 from 'piexifjs'
  export default {
    name: 'Home',
    data() {
      return {
        info: {}
      }
    },
    mounted() {
      window.onload = setTimeout(() => {
        this.startPic()
      }, 1000)

    },
    methods: {
      async startPic() {
        let self = this
        await EXIF.getData(document.getElementById('img1'), cb)
        async function cb() {
          let info = await EXIF.getAllTags(this)
          delete info.MakerNote
          delete info.UserComment
          delete info.thumbnail
          self.info = info
          console.log(self.info)
        }

      },
      async updatedInput(event) {
        let self = this
        await EXIF.getData(event.target.files[0], cb)

        async function cb() {
          let info = await EXIF.getAllTags(this)
          delete info.MakerNote
          delete info.UserComment
          self.info = info
          console.log(self.info)
        }

        if (Object.keys(event).includes('isTrusted')) {
          let reader = new FileReader();
          reader.readAsDataURL(event.target.files[0]);
          reader.onload = async (e) => {
            var image = new Image();
            image.src = e.target.result;
            image.onload = function () {
              document.getElementById('img1').src = image.src;
            }
          }
        }

      },
    }
  }
</script>
<style lang="scss" scoped>
  .list-group {
    text-align: left;
  }
</style>