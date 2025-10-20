<script>
  import { onMount } from "svelte";
  import { page } from "$app/stores";
  import Swal from "sweetalert2";

  let person = {};
  let personId;
  let modal;

  let updatedName = "";
  let updatedEmail = "";
  let updatedAge = "";
  let updatedAddress = "";

  $: personId = $page.params.personId;

  onMount(async () => {
    const res = await fetch(
      `https://person-manager-backend-ten.vercel.app/all-persons/${personId}`,
    );
    person = await res.json();

    // Pre-fill input fields
    updatedName = person.name;
    updatedEmail = person.email;
    updatedAge = person.age;
    updatedAddress = person.address;
  });

  function handleUpdate() {
    modal.showModal();
  }

  async function submitUpdate() {
    modal.close();

    const result = await Swal.fire({
      title: "Are you sure?",
      text: "You are about to update this person's data!",
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
            headers: {
              "Content-Type": "application/json",
            },
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

          // Instant update UI
          person.name = updatedName;
          person.email = updatedEmail;
          person.age = updatedAge;
          person.address = updatedAddress;
        } else {
          throw new Error(data.message || "Update failed!");
        }
      } catch (err) {
        Swal.fire("Error!", err.message || "Something went wrong.", "error");
      }
    }
  }

  async function handleDelete() {
    const result = await Swal.fire({
      title: "Are you sure?",
      text: "This action will permanently delete the person.",
      icon: "warning",
      showCancelButton: true,
      confirmButtonText: "Yes, delete it!",
      cancelButtonText: "Cancel",
    });

    if (result.isConfirmed) {
      try {
        const res = await fetch(
          `https://person-manager-backend-ten.vercel.app/delete-user/${person._id}`,
          {
            method: "DELETE",
          },
        );
        const data = await res.json();
        if (data.deletedCount) {
          await Swal.fire("Deleted!", "Person has been deleted.", "success");
          location.href = "/all-persons";
        } else {
          throw new Error(data.message || "Delete failed.");
        }
      } catch (err) {
        Swal.fire("Error!", err.message || "Something went wrong.", "error");
      }
    }
  }
</script>

<!-- Main Layout -->
<div class="flex justify-center items-center min-h-screen bg-base-200 px-4">
  <div class="card w-full max-w-lg bg-base-100 shadow-2xl p-6">
    <h2 class="text-3xl font-bold mb-4 text-center text-primary">
      Details of {person.name}
    </h2>
    <div class="space-y-4">
      <p class="text-lg">
        👤 <span class="font-semibold text-gray-500">Name:</span>
        <span class="text-gray-200">{person.name}</span>
      </p>
      <p class="text-lg">
        📧 <span class="font-semibold text-gray-500">Email:</span>
        <span class="text-gray-200">{person.email}</span>
      </p>
      <p class="text-lg">
        🎂 <span class="font-semibold text-gray-500">Age:</span>
        <span class="text-gray-200">{person.age}</span>
      </p>
      <p class="text-lg">
        🏠 <span class="font-semibold text-gray-500">Address:</span>
        <span class="text-gray-200">{person.address}</span>
      </p>
    </div>
    <div class="card-actions justify-center mt-6 gap-4">
      <button class="btn btn-accent" on:click={handleUpdate}>✏️ Update</button>
      <button class="btn btn-error" on:click={handleDelete}>🗑️ Delete</button>
    </div>
  </div>

  <!-- DaisyUI modal -->
  <dialog bind:this={modal} class="modal modal-bottom sm:modal-middle">
    <div class="modal-box bg-base-100">
      <h3 class="font-bold text-lg mb-4">Update Person Info</h3>

      <form on:submit|preventDefault={submitUpdate} class="space-y-4">
        <div>
          <label class="label">Name</label>
          <input
            class="input input-bordered w-full"
            type="text"
            bind:value={updatedName}
            required
          />
        </div>
        <div>
          <label class="label">Email</label>
          <input
            class="input input-bordered w-full"
            type="email"
            bind:value={updatedEmail}
            required
          />
        </div>
        <div>
          <label class="label">Age</label>
          <input
            class="input input-bordered w-full"
            type="number"
            bind:value={updatedAge}
            min="0"
            required
          />
        </div>
        <div>
          <label class="label">Address</label>
          <textarea
            class="textarea textarea-bordered w-full"
            bind:value={updatedAddress}
            required
          ></textarea>
        </div>

        <div class="modal-action">
          <button type="submit" class="btn btn-primary">Update</button>
          <button type="button" class="btn" on:click={() => modal.close()}>
            Cancel
          </button>
        </div>
      </form>
    </div>
  </dialog>
</div>
