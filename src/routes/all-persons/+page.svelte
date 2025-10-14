<script>
    import PersonCard from "$lib/components/PersonCard.svelte";
    import { onMount } from "svelte";
    let persons = [];
    let loading = true;
    let error = null;
    onMount(async () => {
        try {
            const res = await fetch("http://localhost:3000/all-persons");
            if (!res.ok) throw new Error("failed to fetch data.");
            persons = await res.json();
        } catch (err) {
            error = err.message;
            alert(error);
        } finally {
            loading = false;
        }
    });
</script>

<h1 class="text-center text-2xl">All persons {persons.length}</h1>
{#if loading}
    <p>loading....</p>
{:else if error}
    <p class="text-red-600">Error: {error}</p>
{:else if persons.length === 0}
    <p>No persons found.</p>
{:else}
    <div
        class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6 max-w-[80%] mx-auto"
    >
        {#each persons as person}
            <PersonCard {person} />
        {/each}
    </div>
{/if}
