<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";
  import 'bootstrap/dist/css/bootstrap.min.css';
  import 'bootstrap/dist/js/bootstrap.bundle.min.js';

  let jsonData = [];
  let tableVisible = false;
  let selectedCandidate = null;
  let editedCandidate = null;
  let isEditing = false;

  const dispatch = createEventDispatcher();

  onMount(async () => {
    await fetchData();
    tableVisible = true;
  });

  async function fetchData() {
    const response = await fetch("https://api.recruitly.io/api/candidate/?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E");
    const responseData = await response.json();
    jsonData = responseData.data;
  }

  function handleTitleClick(candidate) {
    selectedCandidate = candidate;
    editedCandidate = { ...selectedCandidate };
    isEditing = false;
    dispatch("showPopup");
  }

  function handleEdit() {
    isEditing = true;
  }

  async function saveCandidate() {
    Object.assign(selectedCandidate, editedCandidate);

    // Find the index of the selectedCandidate in the jsonData array
    const index = jsonData.findIndex(candidate => candidate.id === selectedCandidate.id);

    // Update the corresponding item in the jsonData array
    jsonData[index] = { ...selectedCandidate };

    // Send the updated candidate data to the API
    await updateCandidate(selectedCandidate);

    isEditing = false;
  }

  async function updateCandidate(candidate) {
    const url = `https://api.recruitly.io/api/candidate/${candidate.id}/?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`;

    // Send a PUT or PATCH request to the API with the updated candidate data
    await fetch(url, {
      method: "PUT", // or "PATCH" depending on the API's requirements
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(candidate),
    });
  }

  function closePopup() {
    selectedCandidate = null;
    editedCandidate = null;
    isEditing = false;
  }
</script>

<style>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup-content {
    background-color: #ffffff;
    padding: 40px;
    border-radius: 10px;
  }
</style>

<main class="container mt-4">
  {#if tableVisible}
    <table class="table">
      <thead class="thead-light">
        <tr>
          <th>First Name</th>
          <th>Surname</th>
          <th>Email</th>
          <th>Mobile</th>
         
        </tr>
      </thead>
      <tbody>
        {#each jsonData as candidate}
          <tr>
            <td>
              <a href="#" on:click|preventDefault={() => handleTitleClick(candidate)}>{candidate.firstName}</a>
            </td>
            <td>{candidate.surname}</td>
            <td>{candidate.email}</td>
            <td>{candidate.mobile}</td>
           
          </tr>
        {/each}
      </tbody>
    </table>
  {/if}
</main>

{#if selectedCandidate}
<div class="popup">
  <div class="popup-content">
    <h1 style="color:blue;">Candidate Details</h1>
    {#if !isEditing}
      <p>First Name: {selectedCandidate.firstName}</p>
      <p>Surname: {selectedCandidate.surname}</p>
      <p>Email: {selectedCandidate.email}</p>
      <p>Mobile: {selectedCandidate.mobile}</p>
      
      <button class="btn btn-primary" on:click={handleEdit}>Edit</button>
    {:else}
      <p>First Name: <input type="text" bind:value={editedCandidate.firstName} /></p>
      <p>Surname: <input type="text" bind:value={editedCandidate.surname} /></p>
      <p>Email: <input type="text" bind:value={editedCandidate.email} /></p>
      <p>Mobile: <input type="text" bind:value={editedCandidate.mobile} /></p>
      
      <button class="btn btn-primary" on:click={saveCandidate}>Save</button>
    {/if}
    <button class="btn btn-secondary" on:click={closePopup}>Close</button>
  </div>
</div>
{/if}
