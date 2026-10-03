<script lang="ts">
import Deficit from '$lib/Deficit.svelte'

let count = $state(2);
let prevCount = 0;

let bool: boolean = $state(false);

let defIssues = $state([]);

function increment() {
    count += 1;
    defIssues = defIssues;
}

function decrement() {
    if(count > 0){
        count -= 1;
        defIssues.pop();
        defIssues = defIssues;
    }
}

let issueCount: number = $derived(defIssues.filter(item => item == true).length)

</script>


<div id="main">
    <div class="component-container">
        <button id="button" onclick={decrement}> - </button>

        <div class="def-container">
            {#each Array(count) as _, index}
            <Deficit bind:defIssue={defIssues[index]} />
            {/each}
        </div>

        <button id="button" onclick={increment}> + </button>
    </div>
    <div id="def-error">
        {#if issueCount >= 2}
            <p>The organisation has recorded deficits exceeding 25% of annual expenditure in the previous two financial years.</p>
        {/if}
    </div>
</div>

<style>
    #main{
        min-height:100vh;
        min-width: 100vw;
        display: flex;
        flex-direction: column;
        font-family: 'Open Sans';
        position: absolute;
    }
    .component-container {
        display: flex;         
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        height: 70vh;
        width: calc(100% - 20px);
    }

    .def-container {
        display: flex;         
        text-align: center;
        overflow: auto;
        overflow-x: auto;
        width:80vw;
        justify-content: center;
        flex-shrink: 0;
    }

    #def-error {
        position: relative;
        text-align: center; 
        font-size: 1.4rem;
        width: 80%;
        left: 10vw;
    }

    #button {
        min-width: 5vw;
        min-height: 5vw;
        padding: 20px 20px; 
        border-radius: 50%;
        flex-shrink: 0;
        font-size: 1rem;
        font-weight: 100;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        top: -5vh;
    }

@media screen and (max-width: 70rem) {
    #main{
        font-size: 1rem;
    }
    .component-container {
        flex-direction: column; 
        height: 80vh;
        font-size: 1rem;
        
    }
    .def-container {
        flex-direction: column; 
        font-size: 1rem;
    }

    #def-error {
        top: 0%;
        font-size: 1rem;
    }
}
</style>