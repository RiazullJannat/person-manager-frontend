<script>
    export let person;
</script>

<div class="card bg-base-100 shadow-xl">
    <div class="card-body">
        <h2 class="card-title">{person.name}</h2>
        <p>{person.email}</p>
        <div class="card-actions justify-end">
            <a
                href={`/all-persons/${person._id}`}
                class="btn btn-primary btn-sm"
            >
                View
            </a>
        </div>
    </div>
</div>
