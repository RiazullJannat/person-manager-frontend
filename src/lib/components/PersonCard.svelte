<script>
    import Swal from "sweetalert2";
    export let person;

    let updatedName = person.name;
    let updatedEmail = person.email;
    let updatedAge = person.age;
    let updatedAddress = person.address;
    let modal;

    async function submitUpdate() {
        modal.close();

        const result = await Swal.fire({
            title: "Are you sure?",
            text: "You're about to update this person's data!",
            icon: "warning",
            showCancelButton: true,
            confirmButtonText: "Yes, update it!",
            cancelButtonText: "Cancel",
        });

        if (result.isConfirmed) {
            try {
                const res = await fetch(
                    `https://person-manager-backend-ten.vercel.app/update-person/${person._id}`,
                    {
                        method: "PATCH",
                        headers: { "Content-Type": "application/json" },
                        body: JSON.stringify({
                            name: updatedName,
                            email: updatedEmail,
                            age: updatedAge,
                            address: updatedAddress,
                        }),
                    },
                );

                const data = await res.json();
                if (data.modifiedCount) {
                    await Swal.fire(
                        "Success!",
                        "Person updated successfully.",
                        "success",
                    );

                    person.name = updatedName;
                    person.email = updatedEmail;
                    person.age = updatedAge;
                    person.address = updatedAddress;
                } else {
                    throw new Error(data.message || "Update failed!");
                }
            } catch (err) {
                Swal.fire(
                    "Error!",
                    err.message || "Something went wrong.",
                    "error",
                );
            }
        }
    }

    async function handleDelete() {
        const result = await Swal.fire({
            title: "Are you sure?",
            text: "This will permanently delete the person.",
            icon: "warning",
            showCancelButton: true,
            confirmButtonText: "Yes, delete it!",
            cancelButtonText: "Cancel",
        });

        if (result.isConfirmed) {
            try {
                const res = await fetch(
                    `https://person-manager-backend-ten.vercel.app/delete-user/${person._id}`,
                    { method: "DELETE" },
                );
                const data = await res.json();

                if (data.deletedCount) {
                    await Swal.fire(
                        "Deleted!",
                        "Person has been deleted.",
                        "success",
                    );
                    location.href = "/all-persons";
                } else {
                    throw new Error(data.message || "Delete failed.");
                }
            } catch (err) {
                Swal.fire(
                    "Error!",
                    err.message || "Something went wrong.",
                    "error",
                );
            }
        }
    }
</script>

<!-- ✨ Clean & Minimal Card -->
<div
    class="bg-base-100 text-base-content rounded-lg p-6 shadow-sm border border-base-300 space-y-4"
>
    <!-- Header -->
    <div class="flex justify-between items-center">
        <h2 class="text-xl font-semibold">{person.name}</h2>

        {#if person.age}
            <span
                class="text-sm rounded px-2 py-1 bg-base-200 text-base-content border border-base-300"
            >
                Age: {person.age}
            </span>
        {/if}
    </div>

    <!-- Info Section -->
    <div class="text-sm space-y-1">
        <div class="flex items-center gap-2">
            <span class="font-medium text-gray-100">Email:</span>
            <span class="text-gray-200">{person.email}</span>
        </div>

        <div class="flex items-center gap-2">
            <span class="font-medium text-gray-100">Address:</span>
            <span class="text-gray-200">{person.address}</span>
        </div>
    </div>

    <!-- Actions -->
    <div class="flex justify-end gap-2 pt-3">
        <a href={`/all-persons/${person._id}`} class="btn btn-sm btn-outline"
            >View</a
        >
        <button
            on:click={() => modal.showModal()}
            class="btn btn-sm btn-outline btn-info">Edit</button
        >
        <button on:click={handleDelete} class="btn btn-sm btn-outline btn-error"
            >Delete</button
        >
    </div>
</div>

<!-- 💬 Minimal DaisyUI Modal -->
<dialog bind:this={modal} class="modal">
    <div class="modal-box bg-base-100 text-base-content">
        <h3 class="font-bold text-lg mb-4">Edit Person</h3>

        <form on:submit|preventDefault={submitUpdate} class="space-y-4">
            <div class="form-control">
                <label class="label">
                    <span class="label-text">Name</span>
                </label>
                <input
                    type="text"
                    class="input input-bordered"
                    bind:value={updatedName}
                    required
                />
            </div>

            <div class="form-control">
                <label class="label">
                    <span class="label-text">Email</span>
                </label>
                <input
                    type="email"
                    class="input input-bordered"
                    bind:value={updatedEmail}
                    required
                />
            </div>

            <div class="form-control">
                <label class="label">
                    <span class="label-text">Age</span>
                </label>
                <input
                    type="number"
                    min="0"
                    class="input input-bordered"
                    bind:value={updatedAge}
                    required
                />
            </div>

            <div class="form-control">
                <label class="label">
                    <span class="label-text">Address</span>
                </label>
                <textarea
                    class="textarea textarea-bordered"
                    bind:value={updatedAddress}
                    required
                ></textarea>
            </div>

            <div class="modal-action">
                <button
                    type="button"
                    class="btn"
                    on:click={() => modal.close()}
                >
                    Cancel
                </button>
                <button type="submit" class="btn btn-primary">Update</button>
            </div>
        </form>
    </div>
</dialog>
