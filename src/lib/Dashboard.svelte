<script>
    import { onMount } from 'svelte';

    import { currentUser, pb } from './pocketbase';
    // Replace with your PocketBase URL

    let data = {
        status: "",
        modelID: "",
        inspectionDay: "",
        location: "",
        timeOfInspection: "",
        customerName: "",
        customerID: "",
        serialNo: "",
        serviceHours: "",
        customerNotes: ""
    };

    let tasks = [];

    onMount(async () => {
        tasks = await getTasks();
    });

    const createTask = async () => {
        await pb.collection('tasks').create(data);
        tasks = await getTasks();
        alert('Task created!');
    };

    const getTasks = async () => {
        return await pb.collection('tasks').getFullList(200, {
            fields: 'id,serialNo,status'
        });
    };

    const updateTaskStatus = async (taskId, status) => {
        await pb.collection('tasks').update(taskId, { status });
        tasks = await getTasks();
    };
</script>

<div class="min-h-screen bg-gray-100 p-8">
    <!-- Task Creation Form -->
    <h1 class="text-2xl font-bold mb-6">Create a Task</h1>
    <form on:submit|preventDefault={createTask} class="space-y-4 mb-12">
        <input type="text" bind:value={data.modelID} placeholder="Model ID" class="input" />
        <input type="datetime-local" bind:value={data.inspectionDay} placeholder="Inspection Day" class="input" />
        <input type="text" bind:value={data.location} placeholder="Location" class="input" />
        <input type="text" bind:value={data.timeOfInspection} placeholder="Time of Inspection" class="input" />
        <input type="text" bind:value={data.customerName} placeholder="Customer Name" class="input" />
        <input type="text" bind:value={data.customerID} placeholder="Customer ID" class="input" />
        <input type="text" bind:value={data.serialNo} placeholder="Serial No" class="input" />
        <input type="text" bind:value={data.serviceHours} placeholder="Service Hours" class="input" />
        <input type="text" bind:value={data.customerNotes} placeholder="Customer Notes" class="input" />
        <button type="submit" class="btn">Create Task</button>
    </form>

    <!-- Task List and Update Status -->
    <h1 class="text-2xl font-bold mb-6">Task List</h1>
    <table class="min-w-full border">
        <thead>
        <tr>
            <th class="border px-4 py-2">Serial No</th>
            <th class="border px-4 py-2">Status</th>
            <th class="border px-4 py-2">Actions</th>
        </tr>
        </thead>
        <tbody>
        {#each tasks as task}
            <tr>
                <td class="border px-4 py-2">{task.serialNo}</td>
                <td class="border px-4 py-2">{task.status}</td>
                <td class="border px-4 py-2">
                    <button on:click={() => updateTaskStatus(task.id, 'complete')} class="text-green-500">Mark as Completed</button>
                </td>
            </tr>
        {/each}
        </tbody>
    </table>
</div>

<style>
    .input {
        @apply block w-full p-2 border rounded;
    }
    .btn {
        @apply bg-blue-500 text-white p-2 rounded;
    }
</style>
