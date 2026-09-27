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
            <p>Organisation has recorded deficits exceeding 25% in the past 2 financial years.</p>
        {/if}
    </div>
</div>

<style>
    #main{
        height:100vh;
        display: flex;
        flex-direction: column;
        font-family: 'Open Sans';
    }
    .component-container {
        display: flex;         
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        height: 70vh;
    }

    .def-container {
        display: flex;         
        text-align: center;
        overflow: auto;
        overflow-x: auto;
        white-space: nowrap;
        width:80vw;
        flex-shrink: 0;
  
        mask-image: linear-gradient(
            to right, 
            transparent, 
            black var(--left-fade), 
            black calc(100% - var(--right-fade)), 
            transparent
        );
        
        scroll-timeline: --scroll-timeline x;
        animation: adjust-fade linear both;
        animation-timeline: --scroll-timeline;
    }

    #def-error {
        position: relative;
        text-align: center; 
        font-size: 1.4rem;
        width: 80%;
        left: 10vw;
    }

    #button {
        width: 5vw;
        height: 5vw;
        border-radius: 50%;
        flex-shrink: 0;
        font-size: 1rem;
        font-weight: 100;
        top: -5vh;
    }

@media (max-width: 768px) {
    .component-container {
        flex-direction: column; 
        height: 80vh;
    }
    .def-container {
        flex-direction: column; 
    }

    #def-error {
        top: 0%
    }
}

@property --left-fade {
    syntax: "<length>";
    inherits: false;
    initial-value: 0px;
}

@property --right-fade {
    syntax: "<length>";
    inherits: false;
    initial-value: 0px;
}

@keyframes adjust-fade {
    0% {
        --left-fade: 0vw;
        --right-fade: 10vw;
    }

    1%, 99% {
        --left-fade: 10vw;
        --right-fade: 10vw;
    }

    100% {
        --left-fade: 10vw;
        --right-fade: 0px;
    }
}
</style>