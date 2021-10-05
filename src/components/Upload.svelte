<script>
    export let files;
    export let pdfUrl; 

    import View from './View.svelte'; 

</script>

<h1>Upload your file or use a link</h1>
<input type="file" bind:files>
<p>
    <input type="text" bind:value={pdfUrl} >
</p>

{#if files}
    <p>You uploaded {files[0].name}</p>
    {:else if pdfUrl}
    <p>You inputed {pdfUrl}</p>
{/if}

<View pdfUrl={pdfUrl} file={files[0]}/> 

