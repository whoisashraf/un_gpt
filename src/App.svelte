<script>
  let text = "";
  let readyForCopying = false;

  function processText() {
    if (text != "") {
      // @ts-ignore
      for (const word in text) {
        text = text.replace(/```(\w+)/g, "");
        text = text.replace("```", "");
        text = text.replace("`", "");
        text = text.replace(/(\*|_){1,2}/g, "");
        text = text.replace(/>/g, "");
        text = text.replace(/\[([^\]]*)\]\([^\)]*\)/g, "$1");
        text = text.replace(/!\[([^\]]*)\]\([^\)]*\)/g, "$1");
        text = text.replace(/\|/g, "");
        // text = text.replace("*", "");
        text = text.replace(/---/g, "");
        text = text.replace(/--/g, "");
        text = text.replace(/#+\s/g, "");
        readyForCopying = true;
        return text.trim();
      }
    }
  }

  function clearText() {
    text = "";
    readyForCopying = false;
  }

  /**
   * @param {{ target: any; }} event
   */
  function copyToClipboard(event) {
    navigator.clipboard.writeText(text);
    let element = event.target;
    element.innerText = "Copied";
    setTimeout(() => {
      element.innerText = "Copy";
    }, 3000);
  }
</script>

<head>
  <title>UnGpt</title>
</head>
<nav>
  <p class="font-bold text-3xl mt-5 text-center">Un_Gpt</p>

  <div>
    <!-- <a href=""></a> -->
  </div>
</nav>



<div class="mx-auto max-w-[900px] mt-1 md:mt-3 text-center">
  <!-- <h1 class="text-2xl">Welcome to Un_Gpt</h1> -->
  <p class=" text-xl opacity-70">Get rid of that weird chatgpt text formatting</p>

  <div class="flex flex-col md:justify-center gap-3">
    <textarea
      class="px-3 pt-3 mx-3 mt-3 placeholder:text-center placeholder:pt-1 placeholder:text-black placeholder:opacity-75 ring-black ring-2 rounded-md bg-blue-50 focus:ring-0"
      bind:value={text}
      name=""
      id=""
      cols="45"
      rows="19"
      placeholder="Paste content from chatgpt here"
    ></textarea>
    <div class="flex gap-3 mx-auto">
      <button
        class="outline outline-black outline-2 bg-blue-50 px-2 rounded-md"
        on:click={clearText}>Clear</button
      >

      {#if readyForCopying}
        <button
          class="bg-black text-white px-2 py-2 rounded-md"
          on:click={copyToClipboard}>Copy</button
        >
      {:else}
        <button
          class="bg-black text-white px-2 py-2 rounded-md"
          on:click={processText}>Strip!</button
        >
      {/if}
    </div>
  </div>
</div>
