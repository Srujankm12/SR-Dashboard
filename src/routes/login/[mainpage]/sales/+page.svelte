<script>
    import Header from "$lib/header.svelte";
import { onMount } from "svelte";

    let checkinStatus = [];
    let showInFields = false;
    let showCheckinFields = false;
    let showCheckoutFields = false;
    let showOutFields = false;
    let showCheckinButton = false;
    let showCheckoutButton = false;
    let showOutButton = false;

    function toggleInFields() {
        showInFields = !showInFields;
    }

    function submitInForm() {
        let timestamp = new Date().toLocaleString();
        checkinStatus = [`Logged in successfully at ${timestamp}`];
        showInFields = false;
        showCheckinButton = true;
    }

    function toggleCheckinFields() {
        showCheckinFields = !showCheckinFields;
    }

    function submitCheckinForm() {
        let timestamp = new Date().toLocaleString();
        checkinStatus = [...checkinStatus, `Checked in at ${timestamp}`];
        showCheckinFields = false;
        showCheckoutButton = true;
    }

    function toggleCheckoutFields() {
        showCheckoutFields = !showCheckoutFields;
    }

    function submitCheckoutForm() {
        let timestamp = new Date().toLocaleString();
        checkinStatus = [...checkinStatus, `Checked out successfully at ${timestamp}`];
        showCheckoutFields = false;
        showOutButton = true;
    }

    function toggleOutFields() {
        showOutFields = !showOutFields;
    }

    function submitOutForm() {
        let timestamp = new Date().toLocaleString();
        checkinStatus = [...checkinStatus, `Logged out successfully at ${timestamp}`];
        showOutFields = false;
    }
</script>

<div class="min-h-screen flex flex-col bg-white text-gray-900">
  <Header />
    
    <main class="flex-grow container mx-auto p-8 py-28">
        <div class="bg-white shadow-2xl rounded-lg p-8 border-t-4 border-black">
            <h2 class="text-2xl font-semibold mb-6 text-black">Sales Team Daily Report Form</h2>

            <div class="mb-4">
                {#each checkinStatus as status}
                    <p class="text-black font-bold">{status}</p>
                {/each}
            </div>

            <button on:click={toggleInFields} class="bg-black text-white font-bold py-2 px-4 rounded shadow-md">
                IN
            </button>

            {#if showInFields}
                <div class="mt-6">
                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mb-2 text-sm font-medium text-gray-700">Work *</label>
                    <select class="w-full border border-gray-300 rounded px-3 py-2 focus:border-black focus:ring-black">
                        <option value="">Select Work</option>
                        <option value="Office">Office</option>
                        <option value="Site">Site</option>
                    </select>

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Today's Work Plan *</label>
                    <textarea class="w-full border border-gray-300 rounded px-3 py-2 focus:border-black focus:ring-black" rows="4"></textarea>

                    <button on:click={submitInForm} class="mt-4 bg-black text-white font-bold py-2 px-4 rounded shadow-md">
                        Submit
                    </button>
                </div>
            {/if}

            {#if showCheckinButton}
                <button on:click={toggleCheckinFields} class="mt-4 bg-green-500 text-white font-bold py-2 px-4 rounded shadow-md">
                    Check-in
                </button>
            {/if}

            {#if showCheckinFields}
                <div class="mt-6">
                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mb-2 text-sm font-medium text-black">Company Name *</label>
                    <select class="w-full border border-gray-300 rounded px-3 py-2 focus:border-black focus:ring-black">
                        <option value="">Select Company</option>
                        <option value="Company A">Company A</option>
                        <option value="Company B">Company B</option>
                    </select>

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Purpose *</label>
                    <textarea class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" rows="4"></textarea>

                    <button on:click={submitCheckinForm} class="mt-4 bg-black text-white font-bold py-2 px-4 rounded shadow-md">
                        Submit
                    </button>
                </div>
            {/if}

            {#if showCheckoutButton}
                <button on:click={toggleCheckoutFields} class="mt-4 bg-red-500 text-white font-bold py-2 px-4 rounded shadow-md">
                    Check-out
                </button>
            {/if}

            {#if showCheckoutFields}
            <div  class=" mt-6">
                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mb-2 text-sm font-medium  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black ">Engineer Name *</label>
                <input type="text" class="w-full border  rounded px-3 py-2  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                
                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">Company Name *</label>
                <input type="text" class="w-full border  rounded px-3 py-2  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                
                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">New/Existing *</label>
                <select class="w-full border  rounded px-3 py-2  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                    <option value="">Select</option>
                    <option value="New">New</option>
                    <option value="Existing">Existing</option>
                </select>
                
                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">Visit On*</label>
                <select class="w-full border  rounded px-3 py-2  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                    <option value="">Select</option>
                    <option value="New">Cold Visit</option>
                    <option value="Existing">Prior Appointment</option>
                </select>
                
                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">Company Sales Stage*</label>
                <select class="w-full border  rounded px-3 py-2  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                    <option value="">Select</option>
                    <option value="New">Order Follow up</option>
                    <option value="Existing">Collaboration</option>
                </select>
                
                <!-- svelte-ignore a11y_label_has_associated_control -->
               
                <label class="block mt-4 mb-2 text-sm font-medium  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">Timeline For Next Action Plan*</label>
                    <input id="report-date" type="date"  class="w-full p-3 border border-gray-300 rounded-md text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-blacks" />
                  
                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium text-black">Challenges *</label>
                <textarea class="w-full border  rounded px-3 py-2 text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black"></textarea>
                
                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">Visit Rating*</label>
                <select class="w-full border  rounded px-3 py-2  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                    <option value="">Select</option>
                    <option value="one">1</option>
                    <option value="two">2</option>
                    <option value="three">3</option>
                    <option value="four">4</option>
                    <option value="five">5</option>
                </select>
                

                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium text-black">Result of the Visit *</label>
                <textarea class="w-full border rounded px-3 py-2  text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black"></textarea>

                <!-- svelte-ignore a11y_label_has_associated_control -->
                <label class="block mt-4 mb-2 text-sm font-medium text-black">Notes *</label>
                <textarea class="w-full border  rounded px-3 py-2 text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black"></textarea>
                </div>
                <button on:click={submitCheckoutForm} class="mt-4 bg-black text-white font-bold py-2 px-4 rounded shadow-md">
                    Submit
                </button>
            {/if}

            {#if showOutButton}
                <button on:click={toggleOutFields} class="mt-4 bg-black text-white font-bold py-2 px-4 rounded shadow-md">
                    OUT
                </button>
            {/if}

            {#if showOutFields}
                <div class="mt-6">
                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Total No Of Visits *</label>
                   <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Total No of Cold Calls *</label>
                    <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Total No of Customer Follow up  *</label>
                    <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Total Enquiry Generate *</label>
                   <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Total Enquiry Generate in Values *</label>
                    <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Total Order Lost *</label>
                    <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                    <!-- svelte-ignore a11y_label_has_associated_control -->
                    <label class="block mt-4 mb-2 text-sm font-medium text-black">Total Order Lost in Values *</label>
                    <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />
 
                     <!-- svelte-ignore a11y_label_has_associated_control -->
                     <label class="block mt-4 mb-2 text-sm font-medium text-black"> Total Order Won *</label>
                     <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />
 
                     <!-- svelte-ignore a11y_label_has_associated_control -->
                     <label class="block mt-4 mb-2 text-sm font-medium text-black">Toatl Order Won in Values *</label>
                     <input type="number" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                     <!-- svelte-ignore a11y_label_has_associated_control -->
                     <label class="block mt-4 mb-2 text-sm font-medium text-black">Customer Name for Follow ups *</label>
                     <input type="text" class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black" />

                     <!-- svelte-ignore a11y_label_has_associated_control -->
                     <label class="block mt-4 mb-2 text-sm font-medium text-black">Notes *</label>
                     <textarea class="w-full border  rounded px-3 py-2 text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black"></textarea>

                     <!-- svelte-ignore a11y_label_has_associated_control -->
                     <label class="block mt-4 mb-2 text-sm font-medium text-black">Tomorrow Goals *</label>
                     <textarea class="w-full border  rounded px-3 py-2 text-black focus:border-black focus:outline-none focus:ring-1 focus:ring-black"></textarea>

                     <!-- svelte-ignore a11y_label_has_associated_control -->
                     <label class="block mt-4 mb-2 text-sm font-medium text-black">How was Today*</label>
                     <select class="w-full border rounded px-3 py-2 focus:border-black focus:ring-black">
                      
                    <option value="one">Select</option>
                        <option value="five">Excellent</option>
                        <option value="one">Good</option>
                        <option value="three">Need to improve</option>
                        </select>


                    <button on:click={submitOutForm} class="mt-4 bg-black text-white font-bold py-2 px-4 rounded shadow-md">
                        Submit
                    </button>
                </div>
            {/if}
        </div>
    </main>
</div>