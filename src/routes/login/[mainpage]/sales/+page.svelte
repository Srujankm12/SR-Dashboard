<script>
    import { onMount } from 'svelte';
    import { page } from '$app/stores';
    import { get } from 'svelte/store';
    import Header from '$lib/header.svelte';

    let userid = '';
    let work = '';
    let todays_work_plan = '';
    let isLoading = false;
    let previousReport = null;
    let showForm = true; // Controls form visibility

    onMount(async () => {
        const urlParams = get(page).params;
        console.log("URL Params:", urlParams);

        if (urlParams && urlParams.mainpage) {
            userid = urlParams.mainpage;
            localStorage.setItem('user_id', userid);
            console.log("User ID from URL:", userid);
        } else {
            const storedUserId = localStorage.getItem('user_id');
            if (storedUserId) {
                userid = storedUserId;
                console.log("User ID from localStorage:", userid);
            } else {
                console.error("User ID is missing from both URL and localStorage!");
            }
        }
        if (userid) {
            await fetchPreviousReport();
        }
    });

    async function fetchPreviousReport() {
        try {
            const response = await fetch(`http://localhost:8000/sales/get/${userid}`);
            
            if (!response.ok) {
                if (response.status === 404) {
                    console.warn("No previous report found.");
                    previousReport = null;
                    showForm = true; // Show form if no report found
                    return;
                }
                throw new Error('Failed to fetch previous report');
            }

            const data = await response.json();
            console.log("Fetched previous report:", data);
            previousReport = { ...data };
            showForm = false; // Hide form if report exists
        } catch (error) {
            console.error('Error fetching previous report:', error);
            previousReport = null;
        }
    }

    function convertToIST(utcDateTime) {
        if (!utcDateTime) return "N/A";
        
        const date = new Date(utcDateTime);
        const options = {
            timeZone: 'Asia/Kolkata',
            year: 'numeric',
            month: 'long',
            day: 'numeric',
            hour: '2-digit',
            minute: '2-digit',
            second: '2-digit'
        };
        
        return new Intl.DateTimeFormat('en-IN', options).format(date);
    }

    async function submitSalesReport() {
        console.log("Submitting with User ID:", userid);

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
            await fetchPreviousReport();
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
            {#if previousReport}
                <div class="mt-4 bg-gray-100 p-4 rounded">
                    <h2 class="font-semibold">Previous Submission:</h2>
                    <p><strong>Login Time:</strong> {convertToIST(previousReport.LoginTime)}</p>
                </div>
                <div class="mt-4 flex space-x-4">
                    <button class="bg-red-500 text-white px-4 py-2 rounded">Logout</button>
                    <button class="bg-black text-white px-4 py-2 rounded">Check-in</button>
                    <button class="bg-black text-white px-4 py-2 rounded">Check-out</button>
                </div>
            {:else}
                <p class="text-gray-500 mt-4">No previous report found for today.</p>
                {#if showForm}
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
            {/if}
        {/if}
    </main>
</div>