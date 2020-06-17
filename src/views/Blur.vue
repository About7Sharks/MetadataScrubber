<template>
    <div>
        <h1>Blur Faces</h1>
        <input id="myFileUpload" type="file" @change="updatedInput">
        <canvas style="width:480px;background:#8080806b" id="my_canvas_id"></canvas>
        
    </div>
</template>
<script>
    import * as faceapi from 'face-api.js';
    // init detection options
    const faceapiOptions = new faceapi.SsdMobilenetv1Options({ minConfidence: 0.6 });

    export default {
        async mounted() {
            console.log(faceapi.nets.ssdMobilenetv1)
            const detectionNet = faceapi.nets.ssdMobilenetv1;
            await faceapi.nets.ssdMobilenetv1.loadFromUri(`/ssd_mobilenetv1_model-weights_manifest.json`)
        },
        methods: {
            async updatedInput(event) {
                
                 const imgFile = document.getElementById('myFileUpload').files[0]
                // create an HTMLImageElement from a Blob
                 const img = await faceapi.bufferToImage(imgFile)
                //  document.getElementById('myImg').src = img.src
                 var myCanvas = document.getElementById('my_canvas_id');
                 myCanvas.width = myCanvas.width

                 var ctx = myCanvas.getContext('2d');
                 ctx.drawImage(img, 0, 0, img.width, img.height, // source rectangle
                0, 0, myCanvas.width, myCanvas.height)
                this.findFaces()
                // let self = this
                // var myCanvas = document.getElementById('my_canvas_id');
                // var ctx = myCanvas.getContext('2d');
                // if (Object.keys(event).includes('isTrusted')) {
                //     let reader = new FileReader();
                //     reader.readAsDataURL(event.target.files[0]);



                //     reader.onload = async (e) => {
                //         console.log(event.target.files)
                //         var image = new Image();

                //         image.src = e.target.result;
                //         image.onload = function () {
                //             console.log(image.width)

                //             ctx.drawImage(image, 0, 0, image.width, image.height, // source rectangle
                //                 0, 0, myCanvas.width, myCanvas.height)

                //             self.findFaces()
                //         }

                //     }
                // }

            },
            async findFaces() {
                var myCanvas = document.getElementById('my_canvas_id');
                 var ctx = myCanvas.getContext('2d');
                const detections = await faceapi.detectAllFaces(myCanvas,faceapiOptions)
                // await faceapi.matchDimensions(myCanvas, document.getElementById(
                //     'my_canvas_id'))
               
                detections.forEach(face=>{
                    // ctx.fillStyle = "#383838";
                    // ctx.filter = 'blur(20px)';
                    ctx.clearRect(face.box.topLeft.x, face.box.topLeft.y, face.box.width, face.box.height);

                }) 
                // await faceapi.draw.drawDetections(myCanvas, faceapi.resizeResults(
                //     detections, myCanvas))
                // console.log(detections)
            }
        }
    }
</script>