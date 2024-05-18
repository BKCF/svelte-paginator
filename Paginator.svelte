<script lang="ts">
    export let items;
    export let elementsPerPage=1;

    $: pageNumber = 0;

    const surroundingButtons:number = 2;
    const nElements:number = items.length;
    $: pageMax = Math.ceil(1.0 * nElements / elementsPerPage);


    
    $: pagerRightPad = ((pageMax - 1) - pageNumber); 
    $: pagerRightButtons = (pagerRightPad >= surroundingButtons) ? surroundingButtons : Math.max(pagerRightPad,0);

    $: pagerLeftPad = (pageMax - 1 - pagerRightPad);
    $: pagerLeftButtons = (pagerLeftPad >= surroundingButtons) ? surroundingButtons : Math.max(pagerLeftPad,0);

    let nPagerButtons:number = 0;
    //We will always display the max # of buttons possible. If some are cut off on the left, or do not exist, give that space to the right, and vice-versa.
    $: {
        if(pagerLeftButtons < surroundingButtons)
        {
            let extraleft = surroundingButtons - pagerLeftButtons;
            pagerRightButtons += extraleft;
            if(1 + 2*surroundingButtons < pagerLeftButtons + pagerRightButtons + 1)
            {
                pagerRightButtons -= (1 + 2*surroundingButtons) - (pagerLeftButtons + pagerRightButtons + 1)
            }
        }
        else if(pagerRightButtons < surroundingButtons)
        {
            let extraright = surroundingButtons - pagerRightButtons;
            pagerLeftButtons += extraright;
            if(1 + 2*surroundingButtons < pagerLeftButtons + pagerRightButtons + 1)
            {
                pagerLeftButtons -= (1 + 2*surroundingButtons) - (pagerLeftButtons + pagerRightButtons + 1)
            }
        }

        nPagerButtons = 1 + pagerLeftButtons + pagerRightButtons;
        //lol not really sure why I need this architecturally....but anyways
        nPagerButtons = Math.min(pageMax, nPagerButtons);
    }
    
    $: pagerStart = pageNumber - pagerLeftButtons;

    $: startIndexForPageItems = pageNumber * elementsPerPage;
    $: itemsOnThisPage = Math.min(nElements - startIndexForPageItems, elementsPerPage);

    function setPage(i:number) : void{
        pageNumber = i;
    }
    function incrPage() : void{
        if (pageNumber + 1 < pageMax){
            pageNumber += 1;
        }
    }
    function decrPage() : void{
        if (pageNumber - 1 >= 0){
            pageNumber -= 1;
        }
    }
</script>


<style>
    body{
        background-color: white;
        font-family: 'Courier New', Courier, monospace;
    }
    div.spaced{
        margin-right: 10%;
    }
    .items-container{
        height: 400px;
    }
    .pager-selected{
        color: white;
        background-color: rgb(165, 112, 182);
    }
    .pager-unselected{
        color: white;
        background-color: rgb(190, 190, 190);
    }
</style>

<div>
    <div class="items-container row d-flex mt-3">
        {#each {length:itemsOnThisPage} as _, i}
            {#if (i+1) % 2 == 0}
                <div class="col-12 d-flex justify-content-center">
                    <div class="spaced">
                        {items[startIndexForPageItems + i - 1]}
                    </div>
                    <div class="">
                        {items[startIndexForPageItems + i]}
                    </div>
                </div>
            {/if}
        {/each}
        {#if (itemsOnThisPage) % 2 == 1}
            <div class="col-12 d-flex justify-content-center">
                <div>
                    {items[startIndexForPageItems + itemsOnThisPage - 1]}
                </div>
            </div>
        {/if}
    </div>
    {pageNumber} {itemsOnThisPage} {startIndexForPageItems} {pagerStart}
    <div class="pager d-flex justify-content-center">
        <div class="d-flex">
            <button disabled={pageNumber==0} on:click={()=>setPage(0)}>|&lt;</button>
            <button style="margin-right: 2px" disabled={pageNumber==0} on:click={()=>decrPage()}>&lt;</button>
            {#each {length:nPagerButtons} as _, i}
                <button style="width:2em; text-align:center;" class={(i+pagerStart == pageNumber) ? "pager-selected" : "pager-unselected"} on:click={()=>setPage(i+pagerStart)}>{i+pagerStart+1}</button>
            {/each}
            <button style="margin-left: 2px" disabled={pageNumber==pageMax-1} on:click={()=>incrPage()}>&gt;</button>
            <button disabled={pageNumber==pageMax-1} on:click={()=>setPage(pageMax-1)}>&gt;|</button>
        </div>
    </div>
</div>