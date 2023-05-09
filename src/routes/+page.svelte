<script>
    import {onMount} from 'svelte';

    let lections = [];
    let errorMessage = '';
    let studentIdVar;
    let lectionIdVar;
    // let server = 'http://localhost:8080';
    let server = 'https://lector-tool.up.railway.app';

    const fetchLections = async () => {
        try {
            const response = await fetch(server+'/api/v1/lections');
            if (response.ok) {
                lections = await response.json();
            } else {
                errorMessage = 'Failed to fetch lections.';
            }
        } catch (error) {
            errorMessage = 'An error occurred while fetching lections.';
        }
    };

    onMount(fetchLections);

    let firstName = '';
    let lastName = '';
    let email = '';

    const addStudent = async () => {
        try {
            const response = await fetch(server+'/api/v1/addStudent', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    firstName,
                    lastName,
                    email
                })
            });

            if (response.ok) {
                const newStudent = await response.json();
                // Do something with the new student, such as add it to the list of students
            } else {
                errorMessage = 'Failed to add student.';
            }
        } catch (error) {
            errorMessage = 'An error occurred while adding the student.';
        }
    };

    const addStudentToLection = async () => {
        try {
            const response = await fetch(server+`/api/v1/addStudentToLection?lectionId=${lectionIdVar}&studentId=${studentIdVar}`, {
                method: 'POST'
            });
            if (response.ok) {
                // Refresh the lections list
                fetchLections();
            } else {
                errorMessage = 'Failed to add student to lection.';
            }
        } catch (error) {
            errorMessage = 'An error occurred while adding student to lection.';
        }
    };
</script>


<h2>All Lections:</h2>

<style>


</style>

{#if errorMessage}
    <p>{errorMessage}</p>
{:else if lections.length > 0}
    <ul>
        {#each lections as lection}
            <li>lectionId: {lection.lectionId} -  {lection.title} - {lection.level} - {new Date(lection.date).toLocaleDateString()}</li>
        {/each}
    </ul>
{:else}
    <p>Loading lections...</p>
{/if}

<h2>Add Student:</h2>

<form on:submit|preventDefault={addStudent}>
    <div>
        <label for="firstName">First Name:</label>
        <input type="text" id="firstName" bind:value={firstName}/>
    </div>

    <div>
        <label for="lastName">Last Name:</label>
        <input type="text" id="lastName" bind:value={lastName}/>
    </div>

    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" bind:value={email}/>
    </div>

    <button type="submit">Add Student</button>
</form>

<form on:submit|preventDefault={addStudentToLection}>
    <div>
        <label for="firstName">StudentId:</label>
        <input type="text" id="studentId" bind:value={studentIdVar}/>
    </div>

    <div>
        <label for="lastName">LectionId:</label>
        <input type="text" id="lectionId" bind:value={lectionIdVar}/>
    </div>


    <button type="submit">Add Student</button>
</form>


