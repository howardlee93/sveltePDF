<script>
    export let files;
    export let pdfUrl; 
    let typed;

    import View from './View.svelte'; 

    const handleUpload = (e) =>{
        let file = e.target.files[0];
        let reader = new FileReader();
        reader.onload = function(){
            let typedarray = new Uint8Array(this.result);
            console.log(typedarray);
            typed = typedarray;
        };
        
        reader.readAsArrayBuffer(file);
    } 

</script>

<h1>Upload your file or use a link</h1>
<input type="file" bind:files on:input={handleUpload}>
<p>
    <input type="text" bind:value={pdfUrl} >
</p>

{#if files}
    <p>You uploaded {files[0].name}</p>
    {:else if pdfUrl}
    <p>You inputed {pdfUrl}</p>
{/if}

<View pdfUrl={pdfUrl} typed={typed}/> 

