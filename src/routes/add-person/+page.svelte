<script>
    // form state variables
    let name = "";
    let email = "";

    async function handleSubmit(event) {
        event.preventDefault(); // prevent default form submission

        const person = { name, email };
        console.log(person);
        try {
            const res = await fetch("http://localhost:3000/add-person", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(person),
            });

            const data = await res.json();

            if (res.ok) {
                alert("Person created successfully!");
                name = "";
                email = "";
            } else {
                alert("Error: " + (data.message || "Something went wrong."));
            }
        } catch (error) {
            console.error("Error:", error);
            alert("Failed to create person. Server error.");
        }
    }
</script>

<h1 class="text-center text-2xl">Add person form below</h1>
<div class="hero bg-base-200 min-h-screen">
    <div class="hero-content flex-col lg:flex-row-reverse">
        <div class="text-center lg:text-left">
            <h1 class="text-5xl font-bold">Create a person now!</h1>
        </div>
        <div class="card bg-base-100 w-full max-w-sm shrink-0 shadow-2xl">
            <form class="card-body" on:submit={handleSubmit}>
                <fieldset class="fieldset">
                    <label class="label">Name</label>
                    <input
                        class="input"
                        bind:value={name}
                        type="text"
                        placeholder="Name"
                        required
                    />
                    <label class="label">Email</label>
                    <input
                        class="input"
                        bind:value={email}
                        type="email"
                        placeholder="Email"
                        required
                    />
                    <button type="submit" class="btn btn-neutral mt-4">
                        Create
                    </button>
                </fieldset>
            </form>
        </div>
    </div>
</div>
