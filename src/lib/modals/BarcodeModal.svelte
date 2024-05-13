<script>
    export let barcode_val;

    const barcodeDetector = new BarcodeDetector({
        formats: ["code_39", "codabar", "ean_13"],
    });

    let videoSource;


    if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
    const constraints = { 
        video: {
            facingMode: 'environment'
        },
        audio: false
    }


    navigator.mediaDevices.getUserMedia(constraints)
    .then(stream => {
        videoSource.srcObject = stream
        videoSource.play();
    });
    }

    const detectCode = () => {
    barcodeDetector.detect(videoSource).then(codes => {
        if (codes.length === 0) 
            return;
        
        for (const barcode of codes)  {
        // Log the barcode to the console
            barcode_val = barcode
        }
    }).catch(err => {
        // Log an error if one happens
        console.error(err);
    })
    }

    setInterval(detectCode, 1000);
</script>

<dialog id="video_modal" class="modal">
    {#if navigator.mediaDevices && navigator.mediaDevices.getUserMedia}
    <div class="modal-box w-11/12 max-w-5xl">
      <h3 class="font-bold text-lg">Hello!</h3>
      <div class="flex justify-center ">
        <video bind:this={videoSource}/>
      </div>
    </div>
    <form method="dialog" class="modal-backdrop">
      <button>close</button>
    </form>
    {:else}
    <h3>Permission denied! Please give access to the camera in order to use the scanning functionality.</h3>
    {/if}
  </dialog>