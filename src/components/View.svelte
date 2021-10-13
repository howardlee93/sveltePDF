<script>
    import * as pdfjsLib from "pdfjs-dist";
    import pdfjsWorker from "pdfjs-dist/build/pdf.worker.entry";  
    let canvasRef;
    // export let pdfUrl;
    export let typed;
    let initialized = false;

    // const data = 'https://www.medicare.gov/Pubs/pdf/10050-medicare-and-you.pdf';
    // const data = './50-writing-tools.pdf';
    // const data = "https://nearlaw.com/PDF/MumbaiHC/2018/2018(7)-ALL-MR-95.pdf";

    // The workerSrc property shall be specified.
    pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker;

    let currentPage = 1; 
    let numPages = 0;
    let pdfRef; 

    const renderPages = (page)=>{
        console.log(currentPage)
        pdfRef && pdfRef.getPage(page)
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

    const load = async (pdfUrl) =>{  
        pdfRef = await pdfjsLib.getDocument(pdfUrl).promise
        initialized = true;
    };

    $:if (typed){
        load(typed)
    };

    $: if(initialized) renderPages(currentPage);

   
</script>

<style>
	.center{
        display:flex;
        justify-content: center;
    }
</style>

{#if typed}
<canvas bind:this={canvasRef} />
{/if}

<div class="center">
    <button on:click={()=>{ if(currentPage > 1){
        currentPage -=1;
        }}
    }> Previous page</button>
    <button on:click={()=>{if(currentPage < numPages){
        currentPage +=1
        }}
    }>Next page</button>
</div>
