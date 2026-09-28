<script lang="ts">

let nzd = new Intl.NumberFormat('en-NZ', {
    style: 'currency',
    currency: 'NZD',
});

let income: number = $state(10);
let expenditure: number = $state(10);
let percent: number = $state(25);

let deficit: number = $derived(income-expenditure);
let limit: number = $derived(expenditure * percent / 100)

let { defIssue = $bindable() } = $props();

$effect(() => {
	if (deficit <= 0 && Math.abs(deficit) >= limit) {
		defIssue = true;
	} else {
		defIssue = false;
	}
});

</script>

<div class="container">
    <div class="def-container">
        <div class="def-column">
            <p>Income</p>
            <input id="input" type="number" bind:value={income} placeholder=123 />
        </div>
        <div class="def-column">
            <p>Expenditure</p>
            <input id="input" type="number" bind:value={expenditure} placeholder=456 />
        </div>
        <div class="def-column">
            <p>Deficit limit (%)</p>
            <input id="input" type="number" bind:value={percent} placeholder=25% />
        </div>
    </div>

    <div id="def-issue">
        {#if defIssue}
            <p>deficit ({nzd.format(deficit)}) is greater than {percent}%.</p>
        {:else}
            <p>no deficit issue.</p>
        {/if}
    </div>
</div>

<style>
    .container {
        display: flex;
        flex-direction: column;
    }
    .def-container {
        display: flex;         
        flex-direction: row;
        padding: 0.5rem 0.25rem;
    }
    .def-column {
        text-align: center;
        font-size: 1.8rem;
        width: 30%;
        padding: 0.0rem 0.25rem;
    }
    #def-issue {
        font-size: 1.4rem;
    }
    input {
        border: 0;
        height: 2em;
        width: 70%;
        font-size: 1.4rem;
    }

    @media (max-width: 768px) {
        input {
            font-size: 1rem;
        }
        #def-issue{
            font-size: 1rem;
        }
        .def-column{
            font-size: 1.4rem;
        }
    }
</style>