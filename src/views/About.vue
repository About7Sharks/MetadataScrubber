<template>
  <div class="about">
    <h1>Metadata Remover</h1>
    <input type="file" id="files" @change="loadPic" />
    <img style='width:480px' src="../assets/IMG_5567.jpeg" id="img1" alt="">
    <p>Right Click image to save without meatadata</p>
  </div>
</template>
<script>
  import piexif from 'piexifjs'
  export default {
    methods: {
      async loadPic(evt) {
        var f = evt.target.files[0]; // FileList object
        let reader = new FileReader();
        reader.readAsDataURL(f);
        reader.onload = async (e) => {
          var image = new Image();
          image.src = e.target.result;
          image.onload = function () {
            document.getElementById('img1').src = piexif.remove(e.target.result);//new img without metadata
          }
     piexif.remove(e.target.result)
        }
       
      }
    }
  }
</script>