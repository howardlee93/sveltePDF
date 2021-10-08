<script>
    import { onMount } from "svelte";
    import * as pdfjsLib from "pdfjs-dist";
    import pdfjsWorker from "pdfjs-dist/build/pdf.worker.entry";  
    let canvasRef;
    export let pdfUrl;
    export let typed;

    $:pdfUrl;

    // const data = 'https://www.medicare.gov/Pubs/pdf/10050-medicare-and-you.pdf';
    // const data = './50-writing-tools.pdf';
    // const data = "https://nearlaw.com/PDF/MumbaiHC/2018/2018(7)-ALL-MR-95.pdf";

    // The workerSrc property shall be specified.
    pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker;


    const load = async (pdfUrl) =>{
        
         pdfjsLib
        .getDocument(pdfUrl).promise
        .then(doc => doc.getPage(1))
        .then(page => {
            const scale = 1.5;
            const viewport = page.getViewport({ scale });

            // Prepare canvas using PDF page dimensions
            var context = canvasRef.getContext('2d');
            canvasRef.height = viewport.height;
            canvasRef.width = viewport.width;

            // Render PDF page into canvas context
            var renderContext = {
                canvasContext: context,
                viewport: viewport
            };

            page.render(renderContext);
        });
    }

    // onMount(async()=>{
    //     if(pdfUrl){
    //         $:load(pdfUrl);
    //     }else if (file.type == 'application/pdf'){

    //         convert(files)
    //         $:load(files);
    //     }
    // })

   $:load(typed);
 
</script>


{#if typed}
    <!-- <p>View: {pdfUrl}</p> -->
    <p> View: {typed}</p>
{/if}
<canvas bind:this={canvasRef} />
