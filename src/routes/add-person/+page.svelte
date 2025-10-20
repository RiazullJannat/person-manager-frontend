<script>
    import Swal from "sweetalert2";
    import { goto } from "$app/navigation";

    let name = "";
    let email = "";
    let age = "";
    let address = "";

    async function handleSubmit(event) {
        event.preventDefault();

        const person = { name, email, age, address };

        try {
            const res = await fetch("https://person-manager-backend-ten.vercel.app/add-person", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(person),
            });

            const data = await res.json();

            if (res.ok) {
                await Swal.fire({
                    icon: "success",
                    title: "Success!",
                    text: "Person created successfully.",
                    timer: 2000,
                    showConfirmButton: false,
                });

                // Clear form
                name = "";
                email = "";
                age = "";
                address = "";

                // Optional redirect
                // goto("/");
            } else {
                Swal.fire({
                    icon: "error",
                    title: "Error!",
                    text: data.message || "Something went wrong.",
                });
            }
        } catch (error) {
            console.error("Error:", error);
            Swal.fire({
                icon: "error",
                title: "Server Error!",
                text: "Failed to create person.",
            });
        }
    }

    function handleCancel() {
        // Just clear the form
        name = "";
        email = "";
        age = "";
        address = "";
    }
</script>

<!-- Form container -->
<div class="flex justify-center items-center min-h-screen bg-gray-900">
    <form
        on:submit={handleSubmit}
        class="bg-gray-800 text-white p-8 rounded-lg shadow-md w-full max-w-md space-y-4"
    >
        <h2 class="text-2xl font-bold">Create New Person</h2>

        <div>
            <label class="block text-sm mb-1">Name *</label>
            <input
                type="text"
                bind:value={name}
                required
                placeholder="Full name"
                class="w-full px-3 py-2 rounded bg-gray-700 border border-gray-600 focus:outline-none focus:ring focus:border-blue-400"
            />
        </div>

        <div>
            <label class="block text-sm mb-1">Email *</label>
            <input
                type="email"
                bind:value={email}
                required
                placeholder="Email"
                class="w-full px-3 py-2 rounded bg-gray-700 border border-gray-600 focus:outline-none focus:ring focus:border-blue-400"
            />
        </div>

        <div>
            <label class="block text-sm mb-1">Age *</label>
            <input
                type="number"
                bind:value={age}
                required
                placeholder="Age"
                class="w-full px-3 py-2 rounded bg-gray-700 border border-gray-600 focus:outline-none focus:ring focus:border-blue-400"
            />
        </div>

        <div>
            <label class="block text-sm mb-1">Address *</label>
            <textarea
                bind:value={address}
                required
                placeholder="Full address"
                class="w-full px-3 py-2 rounded bg-gray-700 border border-gray-600 focus:outline-none focus:ring focus:border-blue-400"
            ></textarea>
        </div>

        <div class="flex justify-start space-x-4 pt-2">
            <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
                Create Person
            </button>
            <button type="button" on:click={handleCancel} class="bg-gray-600 hover:bg-gray-700 text-white px-4 py-2 rounded">
                Cancel
            </button>
        </div>
    </form>
</div>