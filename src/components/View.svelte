<script>
    import * as pdfjsLib from "pdfjs-dist";
    import pdfjsWorker from "pdfjs-dist/build/pdf.worker.entry";  
    let canvasRef;
    export let pdfUrl;
    export let typed;
    let initialized = false;

    // const data = 'https://www.medicare.gov/Pubs/pdf/10050-medicare-and-you.pdf';
    // const data = "https://nearlaw.com/PDF/MumbaiHC/2018/2018(7)-ALL-MR-95.pdf";

    // The workerSrc property shall be specified.
    pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker;

    let currentPage = 1; 
    let numPages;
    let pdfRef; 

    const renderPages = (page)=>{
        console.log(pdfRef);
        numPages = pdfRef.numPages;
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

    const load = async (src) =>{  
        pdfRef = await pdfjsLib.getDocument(src).promise
        // console.log(pdfRef);
        initialized = true;
    };

    $:if (typed || pdfUrl){
        if(typed){
            load(typed);
        }else if(pdfUrl){
            load(pdfUrl);
        }
    };

    $: if(initialized) renderPages(currentPage);

</script>

<style>
	.center{
        display:flex;
        justify-content: center;
    }
</style>

{#if pdfRef}
<canvas bind:this={canvasRef} />
{/if}

<div class="center">
    <button on:click={()=>{ if(currentPage > 1) currentPage --}}> Previous page</button>
    <button on:click={()=> { if(currentPage < numPages)currentPage ++}}>Next page</button>
</div>
{currentPage}
