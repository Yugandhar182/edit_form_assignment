<script>
<<<<<<< HEAD
	import { onMount } from "svelte";
	import { createEventDispatcher } from "svelte";
	import 'bootstrap/dist/css/bootstrap.min.css';
  
	let jsonData = [];
	let tableVisible = false;
	let selectedCandidate = null;
	let editedCandidate = null;
  
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
	  dispatch("showPopup");
	}
  
	function saveCandidate() {
	  Object.assign(selectedCandidate, editedCandidate);
  
	  // Find the index of the selectedCandidate in the jsonData array
	  const index = jsonData.findIndex(candidate => candidate.id === selectedCandidate.id);
  
	  // Update the corresponding item in the jsonData array
	  jsonData[index] = { ...selectedCandidate };
  
	  closePopup();
	}
  
	function closePopup() {
	  selectedCandidate = null;
	  editedCandidate = null;
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
	  padding: 20px;
	  border-radius: 5px;
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
	  <p>First Name:
		<input type="text" bind:value={editedCandidate.firstName} />
	  </p>
	  <p>Surname: <input type="text" bind:value={editedCandidate.surname} /></p>
	  <p>Email: <input type="text" bind:value={editedCandidate.email} /></p>
	  <p>Mobile: <input type="text" bind:value={editedCandidate.mobile} /></p>
	  <button class="btn btn-primary" on:click={saveCandidate}>Save</button>
	  <button class="btn btn-secondary" on:click={closePopup}>Close</button>
	</div>
  </div>
  {/if}
=======
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";
  import 'bootstrap/dist/css/bootstrap.min.css';

  let jsonData = [];
  let tableVisible = false;
  let selectedCandidate = null;
  let editedCandidate = null;

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
    dispatch("showPopup");
  }

  function saveCandidate() {
    Object.assign(selectedCandidate, editedCandidate);

    // Find the index of the selectedCandidate in the jsonData array
    const index = jsonData.findIndex(candidate => candidate.id === selectedCandidate.id);

    // Update the corresponding item in the jsonData array
    jsonData[index] = { ...selectedCandidate };

    closePopup();
  }

  function closePopup() {
    selectedCandidate = null;
    editedCandidate = null;
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
    padding: 20px;
    border-radius: 5px;
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
    <p>First Name:
      <input type="text" bind:value={editedCandidate.firstName} />
    </p>
    <p>Surname: <input type="text" bind:value={editedCandidate.surname} /></p>
    <p>Email: <input type="text" bind:value={editedCandidate.email} /></p>
    <p>Mobile: <input type="text" bind:value={editedCandidate.mobile} /></p>
    <button class="btn btn-primary" on:click={saveCandidate}>Save</button>
    <button class="btn btn-secondary" on:click={closePopup}>Close</button>
  </div>
</div>
{/if}
>>>>>>> 1f452f45cfc33a20d93850aba881189c59677638
