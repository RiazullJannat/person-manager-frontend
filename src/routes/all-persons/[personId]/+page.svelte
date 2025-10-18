<script>
  import { onMount } from "svelte";
  import { page } from "$app/stores";
  import Swal from "sweetalert2";

  let person = {};
  let personId;
  let modal;
  let updatedName = "";
  let updatedEmail = "";

  $: personId = $page.params.personId;

  onMount(async () => {
    const res = await fetch(`http://localhost:3000/all-persons/${personId}`);
    person = await res.json();
    updatedName = person.name;
    updatedEmail = person.email;
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
          `http://localhost:3000/update-person/${person._id}`,
          {
            method: "PATCH",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              name: updatedName,
              email: updatedEmail,
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
          `http://localhost:3000/delete-user/${person._id}`,
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

<div class="flex justify-center items-center min-h-screen bg-base-200 px-4">
  <div class="card w-full max-w-md bg-base-100 shadow-xl">
    <div class="card-body">
      <h2 class="card-title text-3xl text-center">This is {person.name}</h2>
      <div class="space-y-2 text-center mt-4">
        <p class="text-lg font-medium">
          👤 Name: <span class="text-gray-100">{person.name}</span>
        </p>
        <p class="text-lg font-medium">
          📧 Email: <span class="text-gray-100">{person.email}</span>
        </p>
      </div>
      <div class="card-actions justify-end mt-6">
        <button class="btn btn-accent" on:click={handleUpdate}>Update</button>
        <button class="btn btn-error" on:click={handleDelete}>Delete</button>
      </div>
    </div>
  </div>

  <!-- Modal -->
  <dialog bind:this={modal} class="modal modal-bottom sm:modal-middle">
    <div class="modal-box">
      <h3 class="text-lg font-bold">Update Person Information</h3>

      <div class="form-control mt-4">
        <label class="label">Name</label>
        <input
          bind:value={updatedName}
          class="input input-bordered"
          type="text"
        />
      </div>

      <div class="form-control mt-4">
        <label class="label">Email</label>
        <input
          bind:value={updatedEmail}
          class="input input-bordered"
          type="email"
        />
      </div>

      <div class="modal-action">
        <form method="dialog" class="space-x-2">
          <button type="button" class="btn btn-primary" on:click={submitUpdate}
            >Update</button
          >
          <button class="btn">Close</button>
        </form>
      </div>
    </div>
  </dialog>
</div>
