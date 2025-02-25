<script>
    import { onMount } from 'svelte';
    import { page } from '$app/stores';
    import { get } from 'svelte/store';
    import Header from '$lib/header.svelte';

    let userid = '';
    let work = '';
    let todays_work_plan = '';
    let isLoading = false;

    onMount(() => {
        const urlParams = get(page).params;
        console.log("URL Params:", urlParams); // Debugging

        if (urlParams && urlParams.mainpage) {
            userid = urlParams.mainpage; // Extract user_id from URL
            localStorage.setItem('user_id', userid);
            console.log("User ID from URL:", userid);
        } else {
            // Try retrieving from localStorage if not in URL
            const storedUserId = localStorage.getItem('user_id');
            if (storedUserId) {
                userid = storedUserId;
                console.log("User ID from localStorage:", userid);
            } else {
                console.error("User ID is missing from both URL and localStorage!");
            }
        }
    });

    async function submitSalesReport() {
        console.log("Submitting with User ID:", userid); // Debugging

        if (!userid) {
            alert("User ID is missing. Please log in again.");
            return;
        }

        try {
            isLoading = true;
            const response = await fetch('http://localhost:8000/sales/submit', {
                method: 'POST',
               
                body: JSON.stringify({
                    user_id: userid,
                    work,
                    todays_work_plan
                })
            });

            if (!response.ok) {
                throw new Error('Failed to submit report');
            }

            const data = await response.json();
            alert(`Success: ${data.message}\nEmployee ID: ${data.emp_id}`);
        } catch (error) {
            console.error('Error:', error);
            alert('Failed to submit sales report.');
        } finally {
            isLoading = false;
        }
    }
</script>

<div class="min-h-screen flex flex-col bg-white text-gray-900">
    <Header />
    <main class="flex-grow container mx-auto px-6 py-28">
        {#if isLoading}
            <div class="flex justify-center items-center">
                <div class="animate-spin h-12 w-12 rounded-full border-t-4 border-gray-800"></div>
            </div>
        {:else}
            <h1 class="text-xl font-bold">Sales Report Submission</h1>
            <div class="mt-4">
                <label class="block">Work:</label>
                <input bind:value={work} class="border p-2 rounded w-full" />
            </div>
            <div class="mt-4">
                <label class="block">Today's Work Plan:</label>
                <textarea bind:value={todays_work_plan} class="border p-2 rounded w-full"></textarea>
            </div>
            <button on:click={submitSalesReport} class="mt-4 bg-orange-500 text-white px-4 py-2 rounded">
                Submit Report
            </button>
        {/if}
    </main>
</div>
