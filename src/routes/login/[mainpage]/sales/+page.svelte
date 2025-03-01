<script>
    import { onMount } from 'svelte';
    import { page } from '$app/stores';
    import { get } from 'svelte/store';
    import Header from '$lib/header.svelte';

    let userid = '';
    let empid = '';
    let work = '';
    let todays_work_plan = '';
    let isLoading = false;
    let previousReport = null;
    let showForm = true;
    let showLogoutForm = false;
    let logoutSubmitted = false;
    let login_time = '';
    let logoutSummary = null;
    let logoutTime = '';
    let showLogout ='';
    let previousReports ='';

    let logoutData = {
        user_id: '',
        emp_id: '',
        total_no_of_visits: '',
        total_no_of_cold_calls: '',
        total_no_of_follow_ups: '',
        total_enquiry_generated: '',
        total_enquiry_value: '',
        total_order_lost: '',
        total_order_lost_value: '',
        total_order_won: '',
        total_order_won_value: '',
        customer_follow_up_name: "",
        notes: "",
        tomorrow_goals: "",
        how_was_today: "",
        work_location: ""
    };

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
                return;
            }
        }

        if (userid) {
            await fetchPreviousReport();
            await fetchLogoutSummary();
        }
    });

    async function fetchPreviousReport() {
        try {
            const response = await fetch(`http://localhost:8000/sales/get/${userid}`);
            if (!response.ok) {
                if (response.status === 404) {
                    console.warn("No previous report found.");
                    previousReport = null;
                    showForm = true;
                    return;
                }
                throw new Error('Failed to fetch previous report');
            }

            const data = await response.json();
            console.log("Fetched previous report:", data);

            if (data.login_time) {
                data.login_time = convertToIST(data.login_time);
            }

            if (data.emp_id) {
                empid = data.emp_id;
            }

            previousReport = { ...data };
            showForm = false;
        } catch (error) {
            console.error('Error fetching previous report:', error);
            previousReport = null;
        }
    }
    async function fetchLogoutSummary() {
    try {
        const response = await fetch(`http://localhost:8000/sales/getd/${userid}`);
        if (!response.ok) {
            if (response.status === 404) {
                console.warn("No logout summary found.");
                logoutSummary = null;
                logoutTime = localStorage.getItem('logout_time') || ''; // Use stored logout time
                return;
            }
            throw new Error('Failed to fetch logout summary');
        }

        logoutSummary = await response.json();
        console.log("Fetched logout summary:", logoutSummary);

        if (logoutSummary && logoutSummary.logout_time) {
            logoutTime = convertToIST(logoutSummary.logout_time);
            localStorage.setItem('logout_time', logoutTime); // Store logout time persistently
        } else {
            logoutTime = localStorage.getItem('logout_time') || ''; // Retrieve logout time if available
        }
    } catch (error) {
        console.error('Error fetching logout summary:', error);
        logoutSummary = null;
    }
}

    function convertToIST(utcDateTime) {
        if (!utcDateTime) return "N/A";
        const date = new Date(utcDateTime);
        return date.toLocaleString('en-IN', { timeZone: 'Asia/Kolkata' });
    }

    async function submitSalesReport() {
        console.log("Submitting with User ID:", userid);

        if (!userid) {
            alert("User ID is missing. Please log in again.");
            return;
        }

        try {
            isLoading = true;
            const response = await fetch('https://sr-backend-go.onrender.com/sales/submit', {
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
    async function submitLogoutData() {
    console.log("Preparing to submit logout data...");

    if (!userid) {
        userid = localStorage.getItem('user_id') || '';
    }

    if (!previousReport || !previousReport.emp_id) {
        alert("Error: Employee ID is missing. Please refresh the page.");
        return;
    }

    logoutData.user_id = userid;
    logoutData.emp_id = previousReport.emp_id;

    console.log("Submitting logout data:", logoutData);

    try {
        isLoading = true;
        const response = await fetch('https://sr-backend-go.onrender.com/sales/logout', {
            method: 'POST',
            body: JSON.stringify(logoutData)
        });

        if (!response.ok) {
            const errorText = await response.text();
            console.error("Logout API response:", errorText);
            throw new Error('Logout failed');
        }

        const result = await response.json();
        console.log("Logout successful:", result);
        alert("Logout successful");

        // Convert and store logout time
        logoutTime = convertToIST(new Date().toISOString());
        localStorage.setItem('logout_time', logoutTime); // Store logout time persistently
        console.log("Stored Logout Time:", logoutTime);

        // Update UI immediately
        showLogoutForm = false;
        logoutSubmitted = true;
        
        // Re-fetch logout summary to update UI
        await fetchLogoutSummary();
    } catch (error) {
        console.error("Error during logout:", error);
        alert("Logout failed. Check console for details.");
    } finally {
        isLoading = false;
    }
}
function toggleLogoutForm() {
    showLogoutForm = !showLogoutForm;
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
                <p><strong>Logged in at:</strong> {previousReport.login_time || "N/A"}</p>
                
                {#if logoutTime}
                    <p><strong>Logged out at:</strong> {logoutTime}</p>
                {/if}
            </div>
                <div class="mt-4 flex space-x-4">
                    <button class="bg-red-500 text-white px-4 py-2 rounded" on:click={toggleLogoutForm}>Logout</button>


                    <button class="bg-black text-white px-4 py-2 rounded">Check-in</button>
                    <button class="bg-black text-white px-4 py-2 rounded">Check-out</button>
                </div>
                {#if showLogoutForm}
                    <!-- Logout Form -->
                    <div class="mt-6 p-4 border rounded">
                        <h2 class="font-semibold mb-4">Fill Logout Data</h2>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total No. of Visits:</label>
                            <input type="number" bind:value={logoutData.total_no_of_visits} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total No. of Cold Calls:</label>
                            <input type="number" bind:value={logoutData.total_no_of_cold_calls} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total No. of Follow Ups:</label>
                            <input type="number" bind:value={logoutData.total_no_of_follow_ups} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total Enquiry Generated:</label>
                            <input type="number" bind:value={logoutData.total_enquiry_generated} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total Enquiry Value:</label>
                            <input type="number" step="0.01" bind:value={logoutData.total_enquiry_value} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total Order Lost:</label>
                            <input type="number" bind:value={logoutData.total_order_lost} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total Order Lost Value:</label>
                            <input type="number" step="0.01" bind:value={logoutData.total_order_lost_value} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total Order Won:</label>
                            <input type="number" bind:value={logoutData.total_order_won} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Total Order Won Value:</label>
                            <input type="number" step="0.01" bind:value={logoutData.total_order_won_value} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Customer Follow Up Name:</label>
                            <input type="text" bind:value={logoutData.customer_follow_up_name} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Notes:</label>
                            <textarea bind:value={logoutData.notes} class="border p-2 rounded w-full"></textarea>
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Tomorrow Goals:</label>
                            <input type="text" bind:value={logoutData.tomorrow_goals} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">How Was Today:</label>
                            <input type="text" bind:value={logoutData.how_was_today} class="border p-2 rounded w-full" />
                        </div>
                        <div class="mb-2">
                            <!-- svelte-ignore a11y_label_has_associated_control -->
                            <label class="block">Work Location:</label>
                            <input type="text" bind:value={logoutData.work_location} class="border p-2 rounded w-full" />
                        </div>
                        <button on:click={submitLogoutData} class="mt-4 bg-blue-500 text-white px-4 py-2 rounded">
                            Submit Logout Data
                        </button>
                    </div>
                {/if}
            {:else}
                <p class="text-gray-500 mt-4">No previous report found for today.</p>
                {#if showForm}
                    <div class="mt-4">
                        <!-- svelte-ignore a11y_label_has_associated_control -->
                        <label class="block">Work:</label>
                        <input bind:value={work} class="border p-2 rounded w-full" />
                    </div>
                    <div class="mt-4">
                        <!-- svelte-ignore a11y_label_has_associated_control -->
                        <label class="block">Today's Work Plan:</label>
                        <textarea bind:value={todays_work_plan} class="border p-2 rounded w-full"></textarea>
                    </div>
                    <button on:click={submitSalesReport} class="mt-4 bg-orange-500 text-white px-4 py-2 rounded">
                        Submit Report
                    </button>
                {/if}
            {/if}
        {/if}
        {#if logoutSubmitted}
    <div class="mt-4 p-4 bg-green-100 text-green-700 rounded">
        <h2 class="font-semibold">Logout Submitted</h2>
        <p><strong>Logout Time:</strong> {logoutTime}</p>
    </div>
{/if}

    </main>
</div>
